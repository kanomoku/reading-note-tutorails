# git 仓库准备

| 命令                                    | 作用          | 延展阅读 |
| :-------------------------------------- | :------------ | :------- |
| git clone git@github.xxxxxx/testGit.git | clone 仓库    |          |
| git init                                | 初始化git仓库 |          |

# 基础操作

| **命令**                     | 作用                           | 延展阅读 |
| :--------------------------- | :----------------------------- | :------- |
| git status                   | 查看当前路径下所有文件的`状态` |          |
| git status test.txt          | 查看`指定文件test.txt`的状态   |          |
|                              |                                |          |
| git add test.txt             | 添加文件 test.txt 到暂存区     |          |
| git add .                    | 提交当前路径下所有文件到暂存区 |          |
|                              |                                |          |
| git commit test.txt          | 提交`指定文件test.txt`         |          |
| git commit -m "提交全部文件" | 提交`全部文件`                 |          |

# 分支

## 创建分支

| 命令                  | 作用                                                         | 延展阅读 |
| :-------------------- | :----------------------------------------------------------- | :------- |
| git branch test1      | 新建 基于当前分支新建分支test1，但不切换到test1<br />通过 `git branch` 命令创建的分支，只是对某个 `Commit-ID` 的「引用」 |          |
| git checkout -b test2 | 新建 基于当前分支新建分支test2，并切换到test2                |          |

## 创建临时分支

| 命令                                                         | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git checkout e0c619ca3978a38f6eabe79c3dfc67d4296ccc36<br />git switch -c test4 | 新建临时分支`(HEAD detached at e0c619c)`<br />对临时分支创建个新分支`test4` |          |
| git checkout origin/main<br />git switch -c test4            | 新建临时分支 `(HEAD detached at origin/main)`<br />对临时分支创建个新分支`test4` |          |

### 场景1：想要查看某个历史版本的源码

| 命令                                                         | 作用                                                | 延展阅读 |
| :----------------------------------------------------------- | :-------------------------------------------------- | :------- |
| git branch test2 125a1d15e3c07667a0185a636b53b902d2bf81cd    | 新建想要查看某个历史版本的源码<br />新建分支`test2` |          |
| git checkout -b test3 fe5e47ec47e7c4fe300fa65dd7b37c29ddca2251 | 新建想要查看某个历史版本的源码<br />新建分支`test3` |          |

## 查看分支

| 命令           | 作用                                                | 延展阅读 |
| :------------- | :-------------------------------------------------- | :------- |
| git branch     | 查看本地所有分支                                    |          |
| git branch -r  | 查看所有远程分支（`-r 是 --remotes 的简写`）        |          |
| git branch -a  | 查看所有本地分支和远程分支（`-a 是 --all 的简写`）  |          |
| git branch -v  | 查看 `本地分支 + sha1 + commit subject`             |          |
| git branch -vv | 查看 `本地分支 + sha1 + [远程分支] +commit subject` |          |

## 远程分支重命名

| 命令                                                         | 作用                                                         | 延展阅读                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| git checkout old-name<br />git branch -m new-name<br />git push origin :old-name new-name | *# check out the branch*<br />*# change local branch name*<br />*# push local new-name to remote old-name and change the remote branch name* | [链接](https://blog.csdn.net/cuma2369/article/details/107638884) |

## 切换分支

| 命令                | 作用                         | 延展阅读 |
| :------------------ | :--------------------------- | :------- |
| git checkout master | 从当前分支切换到`master`分支 |          |

## 删除分支

| 命令---------------------------------------------------------- | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git branch -d test1                                          | 删除分支`test1`<br />使用 `git branch -d` 删除某个本地分支，只是删除了这个`「引用」`而已，并不会删除任何 `Commit-ID`；但是，如果一个 `Commit-ID` 没有被任何一个分支引用的话，在一定时间之后，将会被 Git 回收机制删除 |          |
| git branch -d -f test2                                       | 强制删除分支`test2`                                          |          |
| git branch -D test3                                          | 强制删除分支`test2`                                          |          |
| git branch -d -r origin/test1                                | 删除分支`origin/test1`                                       |          |

## 远程仓库分支拉取到本地

| 命令                           | 作用                                                   | 延展阅读 |
| :----------------------------- | :----------------------------------------------------- | :------- |
| git checkout -t origin/dev     | 拉取`origin/dev` 到本地并创建`dev`分支 (-t 即 --track) |          |
| git checkout -b dev origin/dev | 拉取`origin/dev` 到本地并创建`dev`分支                 |          |

## 回退版本

| 命令------------------------------------------------------------------------------------------------------------------------ | 作用                                                         | 延展阅读                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| git reset --hard c1a94144cef43d7a795183b04bbad6733aea2eab<br />git reset --hard ORIG_HEAD | 回退提交到`c1a94144cef43d7a79518版本`<br />git reset、git merge、git rebase等危险操作失误时回退到`原来状态` |                                                              |
|                                                              |                                                              |                                                              |
| git reset --mixed c983d4f8da9ab3b8db3d84d2d53e14c56047abc7<br />==<br />git reset c983d4f8da9ab3b8db3d84d2d53e14c56047abc7 | 回退到`提交c983d4` ，并且将回退的代码全部放入到`工作区`中（文件变红） | [链接](https://blog.csdn.net/zts_zts/article/details/115220786) |
| git reset --soft c983d4f8da9ab3b8db3d84d2d53e14c56047abc7    | 回退到`提交c983d4` ，并且将回退的代码全部放入到`暂存区`中（文件变蓝） |                                                              |
| git hard  --soft c983d4f8da9ab3b8db3d84d2d53e14c56047abc7    | 回退到`提交c983d4` ，清空`工作目录`及`暂存区`所有修改（修改内容被直接删除） |                                                              |
| git reset --hard HEAD^                                       | 当前版本回退一个版本                                         |                                                              |
| git reset --hard HEAD^^                                      | 当前版本回退二个版本                                         |                                                              |

### 场景1：撤销工作区的修改（前提是没有增加到暂存区）

| 命令                  | 作用 | 延展阅读 |
| :-------------------- | :--- | :------- |
| git checkout test.txt |      |          |

### 场景2：撤销暂存区的修改

| 命令                                                 | 作用 | 延展阅读 |
| :--------------------------------------------------- | :--- | :------- |
| git reset HEAD test.txt  <br />git checkout test.txt |      |          |

### 场景3：后悔回退操作，恢复到原来的提交

| 命令                                                         | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git reset --hard c1a94144cef43d7a795183b04bbad6733aea2eab<br />git reset --hard ORIG_HEAD | 回退到`c1a941版本`<br />git reset、git merge、git rebase等危险操作失误时回退到`原来状态` |          |

# Fetch

| 命令------------------------------------------------------------- | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git fetch                                                    | 拉取`「所有远程仓库」`所包含的分支到本地，并在本地创建或更新远程分支。所有分支最新的 `Commit-ID` 都会记录在 `.git/FETCH_HEAD` 文件中，若有多个分支，`FETCH_HEAD` 内会多行数据 |          |
| git fetch origin<br />git merge                              | Git 将`远程仓库`下的所有分支拉取到本地的 `refs/remotes/origin/`目录下，`FETCH_HEAD` 设定同上；<br />把 `refs/remotes/origin/` 目录下对应分支合并到 `refs/heads/` 目录下对应分支上 |          |
| git fetch origin main                                        | 拉取 `origin ` 对应远程仓库的 `main` 分支到本地，且 `FETCH_HEAD` 只记录了一条数据，那就是远程仓库 main 分支最新的 `Commit-ID` |          |
| git fetch origin main:temp                                   | 拉取 `origin` 对应远程仓库的 `main` 分支到本地，其中 `FETCH_HEAD` 记录了远程仓库 `main` 分支最新的 `Commit-ID`，并且基于远程仓库的 `main` 分支创建一个名为 temp 的新本地分支（但不会切换至新分支） |          |

# Pull

| 命令                                                         | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git pull <br />`==`<br />git fetch<br />git merge FETCH_HEAD | 拉取`「所有远程仓库」`所包含的分支到本地，然后把本地分支对应的远程分支merge到本地分支 |          |
| git pull origin master<br />`==`<br />git fetch origin master<br />git merge FETCH_HEAD | 拉取 `origin ` 对应远程仓库的 `master` 分支到本地， 然后将其merge到本地分支 |          |

# Rebase

| 命令                      | 作用                               | 延展阅读 |
| :------------------------ | :--------------------------------- | :------- |
| git rebase origin/release | 以`origin/release`的代码为基础变基 |          |

# Merge

| 命令          | 作用                                                         | 延展阅读 |
| :------------ | :----------------------------------------------------------- | :------- |
| git merge dev | `master`分支执行该命令，则把`dev`分支内容`merge`到`master`分支上 |          |

# Cherry-pick

## 基本用法

| 命令--------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| git cherry-pick 125a1d15e3c07667a0185a636b53b902d2bf81cd     | 将指定的提交`commitHash`，应用于当前分支。这会在当前分支产生一个新的提交 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| git cherry-pick branchDev                                    | 将分支`branchDev`，应用于当前分支                            |                                                              |



## 转移多个提交

| 命令--------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git cherry-pick HashA HashB                                  | Cherry pick 支持一次转移多个提交                             |          |
| git cherry-pick A..B                                         | 转移一系列的连续提交（不包含A），提交 A 必须早于提交 B，否则命令将失败，但不会报错 |          |
| git cherry-pick A^..B                                        | 转移一系列的连续提交（包含A）                                |          |

### 场景1：提交`f`应用到`master`分支

| 命令--------------------------------------------------------------------------------------------- | 作用                                         | 延展阅读                                                     |
| :----------------------------------------------------------- | :------------------------------------------- | :----------------------------------------------------------- |
| git checkout master <br /> git cherry-pick f                 | # 切换到 master 分支<br /># Cherry pick 操作 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |

## 配置项

| 命令--------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| git cherry-pick 125a1d15e -e                                 | **`-e`，`--edit`**<br />打开外部编辑器，编辑提交信息。       | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| git cherry-pick 125a1d15e -n                                 | **`-n`，`--no-commit`**<br />只更新工作区和暂存区，不产生新的提交。 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| git cherry-pick 125a1d15e -x                                 | 在提交信息的末尾追加一行`(cherry picked from commit ...)`，方便以后查到这个提交是如何产生的。 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| git cherry-pick 125a1d15e -s                                 | **`-s`，`--signoff`**<br />在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作。 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| git cherry-pick -m 1 125a1d15e                               | **`-m parent-number`，`--mainline parent-number`**<br /><br />如果原始提交是一个合并节点，来自于两个分支的合并，那么 Cherry pick 默认将失败，因为它不知道应该采用哪个分支的代码变动。<br />`-m`配置项告诉 Git，应该采用哪个分支的变动。它的参数`parent-number`是一个从`1`开始的整数，代表原始提交的父分支编号。<br />上面命令表示，Cherry pick 采用提交`commitHash`来自编号1的父分支的变动。<br />一般来说，1号父分支是接受变动的分支（the branch being merged into），2号父分支是作为变动来源的分支（the branch being merged from）。 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |

## 代码冲突

| 命令--------------------------------------------------------------------------------------------- | 作用                                                     | 延展阅读                                                     |
| :----------------------------------------------------------- | :------------------------------------------------------- | :----------------------------------------------------------- |
| Cherry pick操作过程中发生代码冲突，Cherry pick 会停下来，让用户决定如何继续操作<br />用户解决代码冲突<br />将修改的文件重新加入暂存区（`git add .`）<br />git cherry-pick --continue （让 Cherry pick 过程继续执行） |                                                          | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| git cherry-pick --abort                                      | 发生代码冲突后，放弃合并，回到操作前的样子               |                                                              |
| git cherry-pick --quit                                       | 发生代码冲突后，退出 Cherry pick，但是不回到操作前的样子 |                                                              |
|                                                              |                                                          |                                                              |

## 转移到另一个代码库

| 命令--------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git remote add target git://gitUrl<br />git fetch target<br />git log target/master<br />git cherry-pick commitHash | 添加了一个远程仓库`target`<br />远程代码抓取到本地<br />检查一下要从远程仓库转移的提交，获取它的哈希值<br />使用`git cherry-pick`命令转移提交 |          |

# Push

| 命令---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| git push --set-upstream origin main<br />`==`<br />git push origin main <br />git branch --set-upstream-to=origin/main main<br />`==`<br />git push -u origin main | 先把本地分支`push`到远程仓库`.git\refs\remotes\origin`中，然后再建立本地分支与远程分支的关联，即.git/config`里会增加一条`[branch "main"]关联关系； <br />`git push -u`是`git push --set-upstream`的缩写版本<br />第一次推送使用如上命令，以后 `git push` 就可以了 | [链接](https://blog.csdn.net/yzpbright/article/details/115574130) |
| git push origin release                                      | 将特定的分支推向远程仓库`.git\refs\remotes\origin`中，在远程仓库`.git\refs\remotes\origin`中创建特定分支；但是`.git/config`里没有`[branch "release"]` 信息是因为还没有和远程仓库建立起联系<br />如果建立起联系以后执行`git push origin release`则将`所有本地的提交`都发送向`中心仓库` | [链接](https://www.jianshu.com/p/740b219c6546)               |
| git push                                                     | 本地分支和远程仓库建立起联系以后，就可以用 `git push` 推送代码了 |                                                              |
| git push origin --force                                      | 强制进行                                                     |                                                              |
| git push origin --all                                        | 将本地`所有分支`都推送给`特定的远程仓库`                     |                                                              |
| git push origin --tags                                       | 当使用`--all`选项推送所有本地分支时，`tags`并不会被自动推送到远程仓库。因此使用`--tags`选项来向远程仓库推送所有`本地tags` |                                                              |

### 场景1：一套向中心仓库发布本地仓库变更的标准流程

| 命令------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------------------------------------- |
| git checkout main <br />git fetch origin main <br />git rebase -i origin/main <br /># Squash commits, fix up commit messages etc. <br />git push origin main | 1. 切到本地`main`分支;<br />2. 通过`git fetch`同步`中心仓库main分支`在本地的副本，以确保本地副本是最新的;<br />3. 通过`git rebase`操作将分支的修改`变基`到`远程main分支`的`提交历史之上`;（可交互的`rebase`操作（rebase -i）同时也是在分享给其他团队成员之前，清理本地commit记录的好机会）<br />4. `git push`命令将`所有本地的提交`都发送向`中心仓库`；（由于已确保本地的main分支是最新版本的，因此`push`操作是能够快速前进的。此时git不会阻止push操作） | [链接](https://www.jianshu.com/p/740b219c6546) |

### 场景2：--amend 更新上一次提交

| 命令-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | 作用                                                         | 延展阅读                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------------------------------------- |
| # make changes to a repo and git add <br />git commit --amend <br /># update the existing commit message <br />git push --force origin main | 这样的提交通常会`修正并更新commit message`，`或者增加新的修改`。一旦一次commit被修正之后，`git push`会直接失败，因为Git认为修正之后的`commit`与`远程仓库的commit`发生了偏离。修正之后的`commit`需要使用`--force`选项才能推送到远程仓库 | [链接](https://www.jianshu.com/p/740b219c6546) |

### 场景3：删除一个远程分支或者tag

| 命令------------------------------------------------------------------------- | 作用                                                         | 延展阅读                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------------------------------------- |
| git branch -D test3 <br />git push origin :test3             | 有时为了清理仓库或者更好的组织分支结构，需要`对分支进行删除`操作。要想完全删除一个分支，不仅要在`本地删除`，还要在`远程同步删除`。例子里的命令首先删除了名为`test3`的本地分支，然后使用`git push`将这次删除同步给远程仓库。注意这一`push`操作中，会在分支名称前加一个 ：前缀，以便通知远程仓库删除`远端的与本地同名的分支` | [链接](https://www.jianshu.com/p/740b219c6546) |

# log

## 查看log

| 命令                     | 作用                                                         | 延展阅读                                              |
| :----------------------- | :----------------------------------------------------------- | :---------------------------------------------------- |
| git log                  | 输出 commit hsitory with `commit detail`                     |                                                       |
| git log --pretty=oneline | 输出 commit hsitory with `commit id + tag + commit  subject` |                                                       |
| git reflog               | 输出 `HEAD ref 的 reflog`                                    | [链接](https://www.cnblogs.com/onmpw/p/15748110.html) |
|                          |                                                              |                                                       |

## 查看graph

| 命令                                       | 作用                                | 延展阅读 |
| :----------------------------------------- | :---------------------------------- | :------- |
| git log --graph                            | 输出点线图+cimmit信息               |          |
| git log --graph --oneline                  | --oneline标记将每个commit压缩成一行 |          |
| git log --graph --decorate --all           |                                     |          |
| git log --graph --decorate --oneline --all |                                     |          |

---

# Tag

## 创建tag

| **命令**                                                | 作用                                     | 延展阅读 |
| :------------------------------------------------------ | :--------------------------------------- | :------- |
| git tag v1.0                                            | 基于本地当前分支最新commit创建`tag v1.0` |          |
| git tag v2.0 -m "给标签添加说明"                        | 给标签`添加说明`                         |          |
| git tag -a v.0329 -m "message"                          | 创建标签`v.0329`并指定`标签信息`         |          |
| git tag v.0325 125a1d15e3c07667a0185a636b53b902d2bf81cd | 给`指定commit 125a1d`打标签              |          |

## 查看tag

| **命令**                    | 作用                        | 延展阅读 |
| :-------------------------- | :-------------------------- | :------- |
| git show v.0326             | 查看`本地指定tag`的详细信息 |          |
| git tag / git tag -l        | 查看`本地所有tag`           |          |
| git ls-remote --tags origin | 查看`远程所有tag`           |          |

## 删除tag

| **命令**                                                 | 作用                 | 延展阅读 |
| :------------------------------------------------------- | :------------------- | :------- |
| git tag -d v.0325                                        | 删除`本地tag v.0325` |          |
| git tag -d v.0325<br />git push origin :refs/tags/v.0325 | 删除`远程tag v.0325` |          |

## 推送tag

| **命令**               | 作用                                | 延展阅读 |
| :--------------------- | :---------------------------------- | :------- |
| git push origin v1.0   | 推送`本地tag v1.0`到远程仓库        |          |
| git push origin --tags | 推送`全部未推送的本地tag`到远程仓库 |          |

# 远程仓库名称

## 查看远程仓库名称

| **命令**      | 作用                           | 延展阅读 |
| :------------ | :----------------------------- | :------- |
| git remote    | 查看关联的远程分支             |          |
| git remote -v | 查看本地仓库关联的远程仓库信息 |          |

## 修改远程仓库别名

| **命令**                                                     | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git remote add github git@github.com:toFrankie/repo-demo.git | 添加`远程仓库别名`为`github`                                 |          |
| git remote rm github                                         | 删除`远程仓库别名` `github` <br />只会移除本地仓库与远程仓库的关联，不会删除远程仓库的代码 |          |
| git remote rename github gitlab                              | 重命名`远程仓库别名，从 `github` 改为 `gitlab`               |          |
| git remote set-url github git@github.com:toXXXX/repo-demo.git | 给当前别名 `github` 指定一个新的`远程仓库`                   |          |

## 远程仓库默认分支

| **命令**                          | 作用                                                      | 延展阅读 |
| :-------------------------------- | :-------------------------------------------------------- | :------- |
| git remote set-head origin -a     | 将 `origin/HEAD` 设为远程仓库的默认分支（`-a 即 --auto`） |          |
| git remote set-head origin master | 将 origin/HEAD 设为`远程分支master`                       |          |
| git remote set-head origin -d     | 删除`origin/HEAD`                                         |          |

# 设置

## basic

| **命令**                                 | 作用                             | 延展阅读                                                     |
| :--------------------------------------- | :------------------------------- | :----------------------------------------------------------- |
| where git                                | 查询 git 安装路径                |                                                              |
| git --help                               | 帮助文档                         |                                                              |
|                                          |                                  |                                                              |
| git cat-file -p 8ds88f                   | 查看`git的对象的内容`            |                                                              |
|                                          |                                  |                                                              |
| locale                                   | 查看编码设置                     |                                                              |
| export LC_ALL=en_US.UTF-8                | 设置编码属性                     |                                                              |
|                                          |                                  |                                                              |
| git config --global core.quotepath false | 修正 `git bash`中 中文显示为数字 | [链接](https://blog.csdn.net/zhujiangtaotaise/article/details/74424157) |
|                                          |                                  |                                                              |
| clear                                    | 清屏                             |                                                              |

## git config

| **命令**                                      | 作用                                                         | 延展阅读 |
| :-------------------------------------------- | :----------------------------------------------------------- | :------- |
| git config --local -l                         | 查看`仓库级别`                                               |          |
| git config --global -l                        | 查看`全局级别`                                               |          |
| git config --system -l                        | 查看`系统级别`                                               |          |
| git config -l                                 | 查看当前所有级别全部配置信息                                 |          |
|                                               |                                                              |          |
| git config --local -e                         | 编辑`.git/config`文件，会自动调notepad++                     |          |
|                                               |                                                              |          |
| git config --global user.email “xxxxx@qq.com” |                                                              |          |
| git config --global user.name “xxxxx”         |                                                              |          |
| git config --global --add user.age 10         | 增加                                                         |          |
| git config --global --unset user.age          | 删除                                                         |          |
|                                               |                                                              |          |
| git config --global alia.st status            | 给命令配置`别名`，比如用st代替status                         |          |
|                                               |                                                              |          |
| git config –global core.autocrlf true         | git在`push`时 `crlf → lf`，`pull`时 `lf → crlf` ；(默认值)；本地crlf，仓库lf |          |
| git config –global core.autocrlf false        | git在 `push` 和 `pull` 的时候`不转换`；                      |          |
| git config –global core.autocrlf input        | git在`push`时`crlf → lf`，pull时`不转换`；本地lf，仓库lf；（为了消除失误引入的CRLF） |          |
|                                               |                                                              |          |
| git config --global push.default simple       | 不带任何参数的git push， 默认只推送当前分支，这叫做simple方式 |          |
| git config --global push.default matching     | 还有一种叫做matching方式，会推送所有有对应的远程分支的本地分支 |          |









