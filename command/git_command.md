



## git 仓库准备

| 命令                                    | 作用          | 延展阅读 |
| :-------------------------------------- | :------------ | :------- |
| git clone git@github.xxxxxx/testGit.git | clone 仓库    |          |
| git init                                | 初始化git仓库 |          |
|                                         |               |          |

------

## 分支

| 命令                                                         | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| **创建分支**                                                 |                                                              |          |
| git branch test1                                             | 新建 基于当前分支新建分支test1，但不切换到test1<br />通过 `git branch` 命令创建的分支，只是对某个 `Commit-ID` 的「引用」 |          |
| git checkout -b test2                                        | 新建 基于当前分支新建分支test2，并切换到test2                |          |
|                                                              |                                                              |          |
| **创建临时分支**                                             |                                                              |          |
| git checkout e0c619ca3978a38f6eabe79c3dfc67d4296ccc36<br />git switch -c test4 | 新建 临时分支(HEAD detached at e0c619c)<br />新建 对临时分支创建个新分支test4 |          |
| git checkout origin/main<br />git switch -c test4            | 新建 临时分支 (HEAD detached at origin/main)<br />新建 对临时分支创建个新分支test4 |          |
|                                                              |                                                              |          |
| **场景：想要查看某个历史版本的源码**                         |                                                              |          |
| git branch test2 125a1d15e3c07667a0185a636b53b902d2bf81cd    | 新建 想要查看某个历史版本的源码<br />新建 分支test2          |          |
| git checkout -b test3 fe5e47ec47e7c4fe300fa65dd7b37c29ddca2251 | 新建 想要查看某个历史版本的源码<br />新建 分支test3          |          |
|                                                              |                                                              |          |
| **远程仓库分支拉取到本地**                                   |                                                              |          |
| git checkout -t origin/dev                                   | -t 即 --track<br />新建 创建分支dev                          |          |
| git checkout -b dev origin/dev                               | 新建 创建分支dev                                             |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **回退版本**                                                 |                                                              |          |
| git reset --hard c1a94144cef43d7a795183b04bbad6733aea2eab<br />git reset --hard ORIG_HEAD | 回退提交到c1a94144cef43d7a795183b04bbad6733aea2eab<br />git reset、git merge、git rebase等危险操作失误时回退到原来状态 |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **查看分支**                                                 |                                                              |          |
| git branch                                                   | 查看 本地所有分支                                            |          |
| git branch -r                                                | 查看 所有远程分支（-r 是 --remotes 的简写）                  |          |
| git branch -a                                                | 查看 所有本地分支和远程分支（-a 是 --all 的简写）            |          |
| git branch -v                                                | 查看 本地分支 + sha1 + commit subject line                   |          |
| git branch -vv                                               | 查看 本地分支 + sha1 + [远程分支] +commit subject line       |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **切换分支**                                                 |                                                              |          |
| git checkout master                                          | 切换分支到master                                             |          |
|                                                              |                                                              |          |
| **删除分支**                                                 |                                                              |          |
| git branch -d test1                                          | 删除分支test1<br />使用 `git branch -d` 删除某个本地分支，只是删除了这个「引用」而已，并不会删除任何 `Commit-ID`，<br />但是，如果一个 `Commit-ID` 没有被任何一个分支引用的话，在一定时间之后，将会被 Git 回收机制删除。 |          |
| git branch -d -f test2                                       | 强制删除分支test2                                            |          |
| git branch -D test3                                          | 强制删除分支test2                                            |          |
| git branch -d -r origin/test1                                | 删除分支origin/test1                                         |          |
|                                                              |                                                              |          |
| **分支合入**                                                 |                                                              |          |
| master分支执行 git merge dev                                 | 把dev分支内容merge到master分支上                             |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **Fetch**                                                    |                                                              |          |
| git fetch                                                    | 拉取`「所有远程仓库」`所包含的分支到本地，并在本地创建或更新远程分支。<br />所有分支最新的 `Commit-ID` 都会记录在 `.git/FETCH_HEAD` 文件中，若有多个分支，`FETCH_HEAD` 内会多行数据 |          |
| git fetch origin<br />git merge                              | Git 将`远程仓库`下的所有分支拉取到本地的 `refs/remotes/origin/`目录下，`FETCH_HEAD` 设定同上；<br />把 `refs/remotes/origin/` 目录下对应分支合并到 `refs/heads/` 目录下对应分支上 |          |
| git fetch origin main                                        | 拉取 `origin ` 对应远程仓库的 `main` 分支到本地，且 `FETCH_HEAD` 只记录了一条数据，那就是远程仓库 main 分支最新的 `Commit-ID` |          |
| git fetch origin main:temp                                   | 拉取 `origin` 对应远程仓库的 `main` 分支到本地，其中 `FETCH_HEAD` 记录了远程仓库 `main` 分支最新的 `Commit-ID`，并且基于远程仓库的 `main` 分支创建一个名为 temp 的新本地分支（但不会切换至新分支） |          |
|                                                              |                                                              |          |
| **Pull**                                                     |                                                              |          |
| git pull <br /><br />`==`<br />git pull origin 当前分支<br />`==`<br />git fetch<br />git merge FETCH_HEAD | 拉取代码                                                     |          |
| git pull origin master<br />`==`<br />git fetch origin master<br />git merge FETCH_HEAD | 拉取master代码                                               |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **Push**                                                     |                                                              |          |
| git push origin/dev0202                                      |                                                              |          |
| git push -u origin master                                    | 推送master分支代码到Github，可以先merge在推送<br />第一次推送使用此命令 |          |
|                                                              |                                                              |          |
| **Rebase**                                                   |                                                              |          |
| git rebase origin/release                                    | 以远程的代码为基础变基                                       |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **回退版本**                                                 |                                                              |          |
| git reset --hard HEAD^                                       | 当前版本回退一个版本                                         |          |
| git reset --hard HEAD^^                                      | 当前版本回退二个版本                                         |          |
| git reset --hard commitID(对应版本号)                        | 当前版本回退到指定版本                                       |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| 场景：撤销工作区的修改，前提是没有增加到暂存区               |                                                              |          |
| git checkout test.txt                                        |                                                              |          |
|                                                              |                                                              |          |
| 场景：想撤销暂存区的内容需要以下                             |                                                              |          |
| git reset HEAD test.txt  <br />git checkout test.txt         |                                                              |          |
|                                                              |                                                              |          |

------

## 查看log

| 命令                                       | 作用                                                       | 延展阅读                                      |
| :----------------------------------------- | :--------------------------------------------------------- | :-------------------------------------------- |
| **查看log**                                |                                                            |                                               |
| git log                                    | 输出 commit hsitory 包含 commit详细信息                    |                                               |
| git log --pretty=oneline                   | 输出 commit hsitory 包含 commit id + tag + commit  subject |                                               |
| git reflog                                 | 输出 HEAD ref 的 reflog                                    | https://www.cnblogs.com/onmpw/p/15748110.html |
|                                            |                                                            |                                               |
| **查看图**                                 |                                                            |                                               |
| git log --graph                            | 输出 点线图+cimmit信息                                     |                                               |
| git log --graph --oneline                  | --oneline标记将每个commit压缩成一行                        |                                               |
| git log --graph --decorate --all           |                                                            |                                               |
| git log --graph --decorate --oneline --all |                                                            |                                               |

---

## Tag

| **命令**                                                 | 作用                                    | 延展阅读 |
| :------------------------------------------------------- | :-------------------------------------- | :------- |
| **创建tag**                                              |                                         |          |
| git tag v1.0                                             | 创建 本地tag v1.0（当前分支最新commit） |          |
| git tag v2.0 -m"给标签添加说明"                          | 给标签添加说明                          |          |
| git tag -a v.0329 -m "message"                           | 创建 标签并指定标签信息                 |          |
| git tag v.0325 125a1d15e3c07667a0185a636b53b902d2bf81cd  | 给 指定 commit 125a1d打标签             |          |
|                                                          |                                         |          |
| **查看tag**                                              |                                         |          |
| git show v.0326                                          | 查看 本地某个 tag 的详细信息            |          |
| git tag / git tag -l                                     | 查看 本地所有 tag                       |          |
| git ls-remote --tags origin                              | 查看 远程所有 tag                       |          |
|                                                          |                                         |          |
| **删除tag**                                              |                                         |          |
| git tag -d v.0325                                        | 删除本地tag v.0325                      |          |
| git tag -d v.0325<br />git push origin :refs/tags/v.0325 | 删除远程 tag v.0325                     |          |
|                                                          |                                         |          |
| **推送tag**                                              |                                         |          |
| git push origin v1.0                                     | 推送 本地tag v1.0 到远程仓库            |          |
| git push origin --tags                                   | 推送 全部未推送的本地标签到远程仓库     |          |

------

## 远程仓库名称

| **命令**                                                     | 作用                                                         | 延展阅读 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| git remote                                                   | 查看 关联的远程分支                                          |          |
| git remote -v                                                | 查看 本地仓库关联的远程仓库信息                              |          |
|                                                              |                                                              |          |
| **远程仓库别名**                                             |                                                              |          |
| git remote add github git@github.com:toFrankie/repo-demo.git | 添加 github 别名                                             |          |
| git remote rm github                                         | 删除 github 别名<br />只会移除本地仓库与远程仓库的关联，不会删除远程仓库的代码 |          |
| git remote rename github gitlab                              | 重命名 github 为 gitlab                                      |          |
| git remote set-url github git@github.com:toXXXX/repo-demo.git | 重定向 github 别名执行一个新的远程仓库                       |          |
|                                                              |                                                              |          |
|                                                              |                                                              |          |
| **远程仓库默认分支**                                         |                                                              |          |
| git remote set-head origin -a                                | 将 origin/HEAD 设为远程仓库的默认分支（-a 即 --auto）        |          |
| git remote set-head origin master                            | 将 origin/HEAD 设为远程分支master                            |          |
| git remote set-head origin -d                                | 删除 origin/HEAD                                             |          |

------

## 设置

| **命令**                                      | 作用                                                         | 延展阅读 |
| :-------------------------------------------- | :----------------------------------------------------------- | :------- |
| clear                                         | 清屏                                                         |          |
| where git                                     | 查询 git 安装路径                                            |          |
| git --help                                    | 求助                                                         |          |
|                                               |                                                              |          |
| git cat-file -p 8ds88f                        | 查看git的对象内容                                            |          |
|                                               |                                                              |          |
| locale                                        | 查看编码设置                                                 |          |
| export LC_ALL=en_US.UTF-8                     | 设置属性                                                     |          |
|                                               |                                                              |          |
|                                               |                                                              |          |
| git config -l                                 | 查看当前配置信息                                             |          |
| git config --local -l                         | 查看仓库级别                                                 |          |
| git config --global -l                        | 查看全局级别                                                 |          |
| git config --system -l                        | 查看系统级别                                                 |          |
|                                               |                                                              |          |
| git config --local -e                         | 编辑config文件，会自动调notepad++                            |          |
| git config --global user.email “xxxxx@qq.com” |                                                              |          |
| git config --global user.name “xxxxx”         |                                                              |          |
| git config --global --add user.age 10         | 增加                                                         |          |
| git config --global --unset user.age          | 删除                                                         |          |
| git config --global alia.st status            | 给命令配置别名,比如用st代替status                            |          |
|                                               |                                                              |          |
| git config –global core.autocrlf true         | git在`push`时 crlf → lf，`pull`时 `lf → crlf` ；(默认值)；本地crlf，仓库lf |          |
| git config –global core.autocrlf false        | git在 `push` 和 `pull` 的时候`不转换`；                      |          |
| git config –global core.autocrlf input        | git在`push`时`crlf → lf`，pull时`不转换`；本地lf，仓库lf；为了消除失误引入的CRLF |          |
|                                               |                                                              |          |

------

## 基础操作

| **命令**                     | 作用                           | 延展阅读 |
| :--------------------------- | :----------------------------- | :------- |
| git status                   | 查看当前路径下所有文件的状态   |          |
| git status test.txt          | 查看文件 test.txt 的状态       |          |
|                              |                                |          |
| git add test.txt             | 添加文件 test.txt 到暂存区     |          |
| git add .                    | 提交当前路径下所有文件到暂存区 |          |
|                              |                                |          |
| git commit test.txt          | 提交 文件 test.txt             |          |
| git commit -m "提交全部文件" | 提交全部文件                   |          |
|                              |                                |          |
|                              |                                |          |





















