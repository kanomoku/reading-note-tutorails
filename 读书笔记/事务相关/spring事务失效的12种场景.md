

# spring事务失效的12种场景

## 引言

>原文：
>[聊聊spring事务失效的12种场景，太坑了](https://cloud.tencent.com/developer/article/1876768)
>读书笔记：担心大佬文章搬家，故整理此学习笔记

------

spring事务底层使用了aop，也就是通过`jdk动态代理`或者`cglib`，帮我们生成了代理类，在代理类中实现的事务功能。

------

## 一 . 事务不生效

### 1. 访问权限问题

自定义的事务方法），它的访问权限不是`public`，而是`private`、`default`或`protected`的话，`spring`则不会提供事务功能

```java
@Service
public class UserService {
    
    @Transactional
    private void add(UserModel userModel) {
         saveData(userModel);
         updateData(userModel);
    }
}
```



### 2.  方法用final，static修饰

如果某个方法用final修饰了，那么在它的代理类中，就无法重写该方法，而添加事务功能；

```java
@Service
public class UserService {

    @Transactional
    public final void add(UserModel userModel){
        saveData(userModel);
        updateData(userModel);
    }
}
```

如果某个方法用static修饰了，同样无法通过动态代理，变成事务方法；([参考](https://blog.csdn.net/Dax1n/article/details/105684685))

```java
@Service
public class UserService {

    @Transactional
    public static void add(UserModel userModel){
        saveData(userModel);
        updateData(userModel);
    }
}
```



### 3.方法内部调用 

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    @Transactional
    public void add(UserModel userModel) {
        userMapper.insertUser(userModel);
        updateStatus(userModel);
    }

    @Transactional
    public void updateStatus(UserModel userModel) {
        doSameThing();
    }
}
```

从spring容器获取到的`UserService`对象实际是`包装好的proxy对象`，因此调用`add方法`的对象是动态代理对象。而在类内部`add`调用`updateStatus`的过程中，实质执行的代码是`this.updateStatus`，此处this对象是`实际的serviceImpl对象`而不是本该生成的代理对象，因此直接调用了`updateStatus`方法。所以`updateStatus`方法的`@Transactional`不生效。（[参考](https://blog.csdn.net/blacktal/article/details/79345902)）






































