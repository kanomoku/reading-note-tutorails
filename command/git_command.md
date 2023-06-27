# **1. git 仓库准备**

| **命令**                                      | **作用**            | **延展阅读** |
| :-------------------------------------------- | :------------------ | :----------- |
| **`git clone git@github.xxxxxx/testGit.git`** | **clone 仓库**      | **自证**     |
| **`git init`**                                | **初始化 git 仓库** | **自证**     |

# **2. 基础操作**

| **命令**                           | **作用**                             | **延展阅读** |
| :--------------------------------- | :----------------------------------- | :----------- |
| **`git status test.txt`**          | **查看`指定`文件test.txt的状态**     |              |
| **`git status`**                   | **查看当前路径下`所有`文件的状态**   |              |
|                                    |                                      |              |
| **`git add test.txt`**             | **添加`指定`文件 test.txt 到暂存区** |              |
| **`git add .`**                    | **提交当前路径下`所有`文件到暂存区** | **自证**     |
|                                    |                                      |              |
| **`git commit test.txt`**          | **提交`指定`文件test.txt**           |              |
| **`git commit -m "提交全部文件"`** | **提交`全部`文件**                   | **自证**     |

# **3. 分支**

## **3.1 创建分支**

### **场景1：创建新分支** 

**工作中遇到的使用方式：1.拉取新分支，2.做新需求 /修复BUG ，3.基于新分支提PR到目标分支**

| **命令**                              | **作用**                                                     | **延展阅读**        |
| :------------------------------------ | :----------------------------------------------------------- | :------------------ |
| **`git branch test1`**                | **1. 基于当前分支最新提交新建分支 `test1`(但不会切换到`test1`分支)<br />2. 已存在同名分支则报错：`a branch named 'test1' already exists`<br />3. 没有关联的远程分支，故此分支依旧不可以直接执行`git push`** | **自证**            |
| **`git branch test2 125a1d15e`**      | **功能如上<br />区别如下：基于指定提交创建新分支**           | **自证**            |
|                                       |                                                              |                     |
| **`git checkout -b test2`**           | **1. 基于当前分支最新提交新建分支`test2`，并切换到`test2`<br />2. 已存在同名分支则报错：`a branch named 'test2' already exists`<br />3. 没有关联的远程分支，故此分支依旧不可以直接执行`git push`** | **自证<br />☆☆☆☆☆** |
| **`git checkout -b test2 125a1d15e`** | **功能如上<br />区别如下：基于指定提交创建新分支**           | **自证**            |
|                                       |                                                              |                     |
| **`git fetch origin release:dev`**    | **参考下文 `fetch` 详解**                                    |                     |

### **场景2：临时查看某个提交的内容**

**工作中遇到的使用方式：调查BUG时查看git提交历史，临时查看某个提交的代码**

| **命令-------------------------------------------------------------------------------** | **作用**                                                     | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------- |
| **1. `git checkout 125a1d15e`<br /><br />2. `git switch -c test4`** | **1.1. 新建临时分支`(HEAD detached at e0c619c)`<br />1.2.已存在同名分支则报错： `a branch named 'test4' already exists`<br />1.3. 没有关联的远程分支，故此分支依旧不可以直接执行`git push`<br /><br />2. 如果想保留这个临时分支，可执行此命令来创建新分支`test4`** | **自证**     |
|                                                              |                                                              |              |
| **1. `git checkout origin/main`<br />2. `git switch -c test4`** | **功能如上<br />区别如下：拉取远程main分支的最新提交**       | **自证**     |

### **场景3：远程分支拉到本地并 建立关联**

**工作中遇到的使用方式：不通过PR的方式合入代码。直接拉取分支→修改→push**

| **命令**                                                     | **作用**                                                     | **延展阅读**        |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------------------ |
| **`git checkout -t origin/release`**                         | **(-t 即 --track)<br /><br />1. 拉取`origin/release` 到本地并创建`release`分支<br />2. `git push` 直接提交** | **自证**            |
|                                                              |                                                              |                     |
| **`git checkout -b release origin/release`<br /><br />`git push`** | **1. 拉取`origin/release` 到本地并创建`release`分支<br /><br />2. `git push` 直接提交** | **自证<br />☆☆☆☆☆** |
| **`git checkout -b test23 origin/release`<br /><br />`git push origin HEAD:release`** | **1. 拉取`origin/release` 到本地并创建`test23`分支<br /><br />2. `git push` 因为名字不匹配，故报错<br />3.  通过`git push origin HEAD:release` 提交代码** | **自证**            |
|                                                              |                                                              |                     |
| **1. 远程仓库有`test6`分支，本地没有<br />2. `git checkout test6`** | **参考下文 分支切换 详解**                                   | **自证**            |

## **3.2 删除分支**

### **场景1：删除本地分支 or 标签**

| **命令-----------------------------------------------------------------------------------------------------------------** | **作用**                                                     | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------- |
| **`git branch -d test1`**                                    | **只是删除了这个`「引用」`而已，并不会删除任何 `Commit-ID`；<br />但是，如果一个 `Commit-ID` 没有被任何一个分支引用的话，在一定时间之后，将会被 Git 回收机制删除；** | **自证**     |
|                                                              |                                                              |              |
| **`git branch -d -f test2`**                                 | **强制删除分支`test2`**                                      | **自证**     |
| **`git branch -D test3`**                                    | **强制删除分支`test2`**                                      | **自证**     |

### **场景2：删除远程分支 or 标签**

| **命令-----------------------------------------------------------------------------------------------------------------** | **作用**                                                     | **延展阅读**        |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------------------ |
| **1. `git branch -d -r origin/test1`<br />2. `git push origin :test1`** | **1. 删除远程分支在本地的副本`.git/refs/remotes/origin/test1`<br />2. 删除远程分支`test1`<br />注意此`push`操作中，会在分支名称前加一个 ：前缀，以便通知远程仓库删除`远端的与本地同名的分支`** | **自证**            |
|                                                              |                                                              |                     |
| **`git push origin --delete test33`**                        | **删除远程分支`test33`**                                     | **自证<br />☆☆☆☆☆** |

## **3.3 远程分支重命名**

| **命令--------------------------------------------------------------------------** | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **1. `git checkout old-name`<br />2. `git branch -m new-name`<br />3. `git push origin :old-name new-name`** | **1. check out the branch*<br />*2. change local branch name*<br />*3. push local new-name to remote old-name and change the remote branch name** | **[链接](https://blog.csdn.net/cuma2369/article/details/107638884)<br />自证** |

## **3.4 查看分支**

| **命令**             | **作用**                                                     | **延展阅读** |
| :------------------- | :----------------------------------------------------------- | :----------- |
| **`git branch`**     | **查看`所有本地分支`**                                       | **自证**     |
| **`git branch -r`**  | **（`-r 是 --remotes 的简写`）<br /><br />查看`所有远程分支`** | **自证**     |
| **`git branch -a`**  | **（`-a 是 --all 的简写`）<br /><br />查看`所有本地分支+所有远程分支`** | **自证**     |
| **`git branch -v`**  | **举例说明：`test6 828852f msg`**                            | **自证**     |
| **`git branch -vv`** | **举例`test6 62e5e7e [origin/test6] msg`**                   | **自证**     |

## **3.5 切换分支**

| **命令**                  | **作用**                         | **延展阅读** |
| :------------------------ | :------------------------------- | :----------- |
| **`git checkout master`** | **从当前分支切换到`master`分支** | **自证**     |

### **场景1：远程仓库有test6分支，本地没有同名分支，本地`git checkout test6` 会自动拉取远程分支test6并创建同名本地分支**

| **命令------------------------------------------------------------------------------------------------------------------------** | **作用**                                                     | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------- |
| **1. 远程仓库有`test6`分支，本地没有<br />2. `git checkout test6`** | **1. 自动拉取`远程分支test6` 并创建`同名本地分支`<br />2. `.git/config`配置文件里会追加关联关系 `[branch "test6"]`，此分支可以直接执行`git push`** | **自证**     |
| **1. 远程仓库有`test6`分支，本地有`test6`，他们没有关联关系，恰巧重名时<br />2. `git checkout test6`** | **`git push` 会被拦截<br />具体如何提交参考 `push` 命令**    |              |

# **版本回退**

| **命令----------------------------------------------------------** | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **`git reset --mixed c983d4`<br />`==`<br />`git reset c983d4`** | **回退到`提交c983d4版本` ，并且将回退的代码全部放入到`工作区`中（文件变红）** | **[链接](https://blog.csdn.net/zts_zts/article/details/115220786)<br />自证** |
| **`git reset --soft c983d4`**                                | **回退到`提交c983d4版本` ，并且将回退的代码全部放入到`暂存区`中（文件变蓝）** | **自证**                                                     |
| **`git reset --hard c983d4`**                                | **回退到`提交c983d4版本` ，清空`工作目录`及`暂存区`所有修改（修改内容被直接删除）** | **自证**                                                     |

### **场景1：回退到指定提交**

| **命令**                          | **作用**                                                     | **延展阅读** |
| :-------------------------------- | :----------------------------------------------------------- | :----------- |
| **`git reset --hard c983d4f8d`**  | **回退到`提交c983d4f8d版本`**                                | **自证**     |
| **`git reset --hard ORIG_HEAD`**  | **`git reset`、`git merge`、`git rebase`等危险操作失误时回退到`原来版本状态`** | **自证**     |
| **`git reset --hard HEAD`**       | **回退到当前分支最新提交**                                   | **自证**     |
| **`git reset --hard HEAD^`**      | **回退到当前分支最新提交的上`1`个提交**                      | **自证**     |
| **`git reset --hard HEAD^^`**     | **回退到当前分支最新提交的上`2`个提交**                      | **自证**     |
| **`git reset --hard HEAD^^^^^^`** | **回退到当前分支最新提交的上`^个数`个提交**                  | **自证**     |

### **场景2：撤销工作区的修改（还未加到暂存区，即当前文件颜色为红色时）**

| **命令**                    | **作用**         | **延展阅读** |
| :-------------------------- | :--------------- | :----------- |
| **`git checkout test.txt`** | **撤销单个文件** | **自证**     |
| **`git checkout .`**        | **撤销全部文件** | **自证**     |

### **场景3：撤销暂存区的修改（当前文件颜色为蓝色时）**

| **命令**                                                     | **作用**                                               | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------- | :----------- |
| **1. `git reset HEAD test.txt`  <br />2. `git checkout test.txt`** | **1. 恢复指定文件到工作区<br />2. 撤销指定文件的修改** | **自证**     |
| **1. `git reset HEAD .`<br />2. `git checkout .`**           | **功能如上<br />区别如下：全部文件**                   | **自证**     |

# **4. Fetch**

| **命令-----------------------------------------------------------------------------** | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **`git fetch`**                                              | **1. 拉取「`远程仓库`」的`所有远程分支`的`最新Commit-ID` 记录在 `.git/FETCH_HEAD` 文件中.<br />若有多个分支则 `FETCH_HEAD` 内会有多行数据，该`文件首行`对应的是 `git fetch` 时`所在分支`的`同名远程分支`<br /><br />2. 远程仓库被`clone`到本地后，被`push`过代码的远程分支会在`.git\refs\remotes\origin` 路径下`创建或更新`其在本地的副本** | **1. [参考1](https://blog.csdn.net/weixin_37646636/article/details/130412396)<br /><br />2. [参考2](https://blog.csdn.net/weixin_37646636/article/details/130403890)<br />自证** |
| **`git fetch origin`**                                       | **功能如上**                                                 | **自证<br />☆☆☆☆☆**                                          |
| **`git fetch origin release`**                               | **功能如上<br />区别如下：`FETCH_HEAD` 内只有`1`行数据，记录的是 `git fetch` 时`指定的远程分支`的`最新Commit-ID`** | **自证<br />☆☆☆☆☆**                                          |
|                                                              |                                                              |                                                              |
| **`git fetch origin release:dev`**                           | **1. 使用`远程release分支`在本地创建`本地dev分支`(但不会切换到该分支)。<br />如果不存在`本地dev分支`，则会自动创建一个新的`本地dev分支`；<br />如果存在`本地dev分支`，并且满足`fast forward`条件, 则`自动合并`两个分支，否则，会`阻止`以上操作。<br /><br />2. 新分支和远程分支为衍生关系，故不存在关联。** |                                                              |
| **`git fetch origin :branch2`<br />`==`<br />`git fetch origin master:branch2`** | **此时默认 `master` 为远程仓库默认分支**                     |                                                              |

# **5. Merge**

## **场景1：本地分支间合并

| **命令**            | **作用**                                                     | **延展阅读** |
| :------------------ | :----------------------------------------------------------- | :----------- |
| **`git merge dev`** | 在`master`分支执行该命令，则把`dev`分支内容`merge`到`master`分支上 | **自证**     |

## **场景2：`远程release分支`合并到`本地dev分支`**

| **命令**                                                     | **作用**                       | **延展阅读**        |
| :----------------------------------------------------------- | :----------------------------- | :------------------ |
| **方式1**：最省事方式                                        |                                |                     |
| 1. `git checkout dev`<br />**2. `git pull`<br />**`==`<br />1. `git checkout dev`<br />2. `git fetch`<br />3. `git merge` | **`origin/dev` ☞ `heads/dev`** | **自证**            |
|                                                              |                                |                     |
| **方式2**：最省事+最严谨+最高效                              |                                |                     |
| 1. `git checkout dev`<br />2. **`git pull origin release`**<br />`==`<br />1. `git checkout dev`<br />2. `git fetch origin release`<br />3. `git merge origin/release` |                                | **自证<br />☆☆☆☆☆** |
| **方式2的简化**：会浪费些性能                                |                                |                     |
| 1. `git checkout dev`<br />2. `git fetch origin`<br />3. `git merge origin/release` |                                | **自证<br />☆☆☆☆☆** |
|                                                              |                                |                     |
| **方式3**：基于本地分支之间merge                             |                                |                     |
| **1. `git checkout release`<br />2. `git pull`<br /><br />3. `git checkout dev`<br />4. `git merge release`** |                                | **自证**            |
|                                                              |                                |                     |
| **方式4**：主要需知道FETCH_HEAD含义，此方法太浪仅做了解即可  |                                |                     |
| **1. `git checkout dev`<br />2. `git fetch origin release`<br />3. `git merge FETCH_HEAD`** |                                | **自证**            |

## **场景3：`git merge origin master`和`git merge origin/master`的区别**

| **命令**                                                     | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **1. `git checkout dev`<br />2. `git fetch origin master` <br />3. `git merge origin release`<br />==<br />1. `git checkout dev`<br />2. `git fetch origin master` <br />3. `git merge origin/master release` | 前提：`master`为`origin`默认分支<br />1. 拉取最新`origin/master`<br />2. 把`origin/master` 分支和`heads/release`分支merge ☞ `heads/dev` | **[链接](https://blog.csdn.net/lansetudou/article/details/116841262)** |

## **场景4：`merge`发生冲突时☞撤销merge**

| **命令**                                                     | **作用**  | **延展阅读** |
| :----------------------------------------------------------- | :-------- | :----------- |
| 1. `git checkout dev`<br />2. `git pull origin release`<br />3. 发生冲突<br /><br /><br />方式1：`git reset --hard HEAD`<br /><br />方式2：`git reset --hard 8ec554`<br /><br />方式3： **`git merge --abort`** | 撤销merge | 自证         |

## **场景5：`merge`发生冲突时☞解决冲突**

| **命令**                                                     | **作用**                                                     | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------- |
| **1. `git checkout release`<br />2. `git pull origin release`<br />**<br />3. 本地解决冲突<br /><br />**4. `git add .`<br />5. `git commit -m "Msg"`<br />6. `git push`** | 把`origin/release`合入`本地release分支`<br />然后推送`本地release分支`到远程仓库 |              |
| **1. `git checkout dev`<br />2. `git fetch`<br />3. `git merge origin/release`<br />**<br />4. 本地解决冲突<br /><br />5. `git add .`<br />6. `git commit -m "Msg"`<br />7. `git push` | 把`origin/release`合入`本地dev分支`<br />然后推送`本地dev分支`到远程仓库 |

## **场景6：`版本V0118分支`开发到一定阶段时，把`版本release分支`回合到`版本V0118分支`**

| **命令**                                                     | **作用**                   | **延展阅读**                                                 |
| :----------------------------------------------------------- | :------------------------- | :----------------------------------------------------------- |
| 1. `git fetch`<br />2. `git checkout -b v0118 origin/release`<br />3. `date >> 1.txt && git add . && git commit -m "msg"` ☞ 模拟`v0118分支`提交业务代码<br /><br />期间`远程release分支`被合入诸多别的业务模块的代码提交<br /><br />`v0118分支`开发到一定阶段时，维护性的把`release分支`回合到 `v0118分支`<br /><br />1. `git fetch origin release v0118` ☞ `fetch`两个分支对应远程分支的最新提交<br />2. `git checkout -b release origin/release` ☞ 拉取`release分支`<br />3. `git merge origin/v0118` ☞ `v0118分支`合入`release分支`<br />4. 解决冲突<br />5. `git push origin HEAD:v0118` ☞ `合入v0118分支的release分支`充当`远程v0118分支` | **此行为可保护好提交历史** | **[参考](https://blog.csdn.net/u010312474/article/details/107915694)<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />超神推荐** |

# **6. Rebase**

| **命令**                        | **作用**                               | **延展阅读** |
| :------------------------------ | :------------------------------------- | :----------- |
| **`git rebase origin/release`** | **以`origin/release`的代码为基础变基** | **自证**     |

## **场景1：一次基于 `rebase` 的代码提交**

| **命令**                                                     | **作用** | **延展阅读** |
| :----------------------------------------------------------- | :------- | :----------- |
| 1. `git fetch`<br />2. `git checkout -b dev2 origin/dev2`<br />3. `date >> 1.txt && git add . && git commit -m "msg"`  ☞ 模拟业务提交<br /><br />期间`远程release分支`被合入诸多别的业务模块的代码提交<br /><br />`dev2分支`往`release分支`提PR之前，先rebase一下<br /><br />1. `git fetch origin` ☞ 拉取最新代码<br />2. **`git rebase origin/release`** ☞ 执行rebase<br />3. **`git push -u origin dev2`** ☞ push代码然后github提PR |          | **自证**     |

## **场景2：`rebase`发生冲突时 ☞ 撤销rebase**

| **命令**                 | **作用**       | **延展阅读** |
| :----------------------- | :------------- | :----------- |
| **`git rebase --abort`** | **撤销rebase** | **自证**     |

## **场景3. 员工A提PR ☞ 员工B合入PR到release ☞ 员工C revert PR ☞ 员工A 在提PR的 fix-bug分支 `rebase`  origin/release ☞ rebase后修改内容没了，如何再重提这个PR呢？**

| **命令**                                                     | **作用**                                                     | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------- |
| **因为git认为变化过程是`原来内容→追加内容→删除内容`，<br />此时`rebase`时，删除动作视为最新提交所以会把本地修改给清除，<br />如何再重提这个PR呢？** |                                                              |              |
| **1. `git reflog`<br />2. `git fetch`<br />3. `git reset --hard origin/release`<br />4. `git cherry-pick 542a43`<br />5. `git branch -vv`<br />6. `git push origin HEAD:fix-bug -f`** | **1. 找到员工A提PR的那个提交`542a43`<br />2. 拉取最新代码<br />3. 本地使用`远程release分支`代码<br />4. 把`542a43`内容`cherry-pick`过来<br />5. 为了查看本地分支对应的远程分支的名字`fix-bug`<br />6. 把本地分支强推到远程的`fix-bug`上** | **自证**     |

# **7. Pull**

| **命令**                                                     | **作用**                                             | **延展阅读**                                                 |
| :----------------------------------------------------------- | :--------------------------------------------------- | :----------------------------------------------------------- |
| `git pull` <br />`==`<br />1. `git fetch`<br />2. `git merge FETCH_HEAD` | 1. `fetch` origin的所有分支<br />2. `merge` 当前分支 | [参考](https://blog.csdn.net/weixin_37646636/article/details/129792724) |
| **`git pull origin master`<br />`==`<br />1. `git fetch origin master`<br />2. `git merge FETCH_HEAD`** | **1. `fetch` 当前分支<br />2. `merge` 当前分支**     |                                                              |

# **8. Cherry-pick**

## 8.1 基本用法

| **命令**                     | **作用**                                                     | **延展阅读**                                                 |
| :--------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **`git cherry-pick 125a1d`** | **将提交`125a1d`应用于`当前分支`. 在`当前分支`会`产生一个新的提交`.** | **[链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)** |
| **`git cherry-pick bugfix`** | **将分支`bugfix`应用于当前分支. 在`当前分支`会`产生一个新的提交`.** |                                                              |

### **场景1：提交`125a1d`应用到`master`分支**

| **命令**                                                     | **作用** | **延展阅读**                                                 |
| :----------------------------------------------------------- | :------- | :----------------------------------------------------------- |
| **1. `git checkout master` <br />2. `git cherry-pick 125a1d`** |          | **[链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)** |

## **8.2 cherry-pick多个提交**

| **命令**                            | **作用**                                                     | **延展阅读** |
| :---------------------------------- | :----------------------------------------------------------- | :----------- |
| **`git cherry-pick 125a1d 125a2a`** | **cherry pick 支持一次`转移多个提交`**                       |              |
| **`git cherry-pick A..B`**          | **转移一系列的`连续提交（不包含A）`，提交 A 必须早于提交 B，否则命令将失败，但不会报错** |              |
| **`git cherry-pick A^..B`**         | **转移一系列的`连续提交（包含A）`**                          |              |

## **8.3 Cherry-pick配置项**

| **命令**                       | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `git cherry-pick 125a1d -e`    | `-e`，`--edit`<br />打开外部编辑器，编辑提交信息             | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| `git cherry-pick 125a1d -x`    | 在提交信息的末尾追加一行`(cherry picked from commit ...)`，<br />方便以后查到这个提交是如何产生的 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| `git cherry-pick 125a1d -s`    | `-s`，`--signoff`<br />在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
|                                |                                                              |                                                              |
| `git cherry-pick 125a1d -n`    | `-n`，`--no-commit`<br />只更新工作区和暂存区，不产生新的提交 | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |
| `git cherry-pick -m 1 125a1d ` | `-m parent-number`，`--mainline parent-number`<br />如果原始提交是一个`合并节点`，来自于两个分支的合并，<br />那么 Cherry pick 默认将失败，因为它不知道应该采用哪个分支的代码变动。<br /><br />`-m`配置项告诉 Git，应该采用哪个分支的变动。参数`parent-number`是一个从`1`开始的整数<br />`1`号父分支是`接受变动的分支`（the branch being merged into），<br />`2`号父分支是作为`变动来源的分支`（the branch being merged from） | [链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html) |

## **8.4 cherry-pick过程中代码冲突**

| **命令**                                                     | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **cherry pick操作过程中发生代码冲突，<br />Cherry pick 会停下来，让用户决定如何继续操作** |                                                              |                                                              |
| **1. 用户解决代码冲突;<br />2. `git add .`（将修改的文件重新加入暂存区）<br />3. `git cherry-pick --continue` （让 cherry pick 过程继续执行）** |                                                              | **[链接](https://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)** |
| **`git cherry-pick --abort`**                                | **发生代码冲突后，放弃合并，回到操作前的样子**               |                                                              |
| **`git cherry-pick --quit`**                                 | **发生代码冲突后，退出 Cherry pick，但是不回到操作前的样子** |                                                              |

## **8.5 Cherry-pick到另一个代码库**

| **命令**                                                     | **作用**                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| **1. `git remote add target git@github.xxxxxx/testGit.git`<br />2. `git fetch target`<br />3. `git log target/master`<br />4. `git cherry-pick commitHash`** | **1. 添加了一个远程仓库`target`<br />2. 远程代码抓取到本地<br />3. 获取要从远程仓库转移的提交，获取它的哈希值`commitHash`<br />4. 使用`git cherry-pick`命令转移提交** |

# **9. Push**

| **命令**                                                     | **作用**                                                     | **延展阅读**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `git push --set-upstream origin release`<br /><br />`==`<br /><br />**`git push -u origin release`** 为缩写版本<br /> | 1. `.git/config`配置文件会追加如下关联关系，<br />[branch "release"]<br/>	remote = origin<br/>	merge = refs/heads/release<br />故后续可以直接执行`git push`<br /><br />2. `.git\refs\remotes\origin` 里会追加文件`release` | [链接](https://blog.csdn.net/yzpbright/article/details/115574130)<br />自证 |
|                                                              |                                                              |                                                              |
| 1. **`git branch --set-upstream-to=origin/release2 release3`**<br /><br />2. **`git push origin HEAD:release2`** | `本地release3分支`和远程`origin/release2分支`建立关联<br />`branch 'release3' set up to track 'origin/release2'.`<br /><br />`.git/config` 新增关系<br />[branch "release3"]<br/>	remote = origin<br/>	merge = refs/heads/release2<br /><br />分支名不同时push代码的方式 | 自证                                                         |
|                                                              |                                                              |                                                              |
| `git push`                                                   | `本地分支`和`远程分支`建立起联系后，<br />就可以用 `git push` 直接推送代码；<br /><br />关联关系体现在 ☞ `.git/config` 里有关联关系<br />[branch "release"]<br/>	remote = origin<br/>	merge = refs/heads/release | 自整                                                         |
|                                                              |                                                              |                                                              |
| `git push origin release`                                    | 1. `本地release`和`远程release`满足`fast-forward`则可以合入<br />2. `本地release`和`远程release`不满足`fast-forward`则被报错拦截<br />3. `.git/config`中未追加关联关系也可执行此操作；<br />4. `.git\refs\remotes\origin` 里会追加文件 `release` | [链接](https://www.jianshu.com/p/740b219c6546)<br />自证     |
|                                                              |                                                              |                                                              |
| `git push` 和`git push origin release ` 区别                 | 1. 当只关联一个远程，只有一个分支时，这两个命令没什么区别。<br />2. 当关联了两个多个仓库、有多个分支时，`git push`可能会报错，<br />因为它不知道要上传代码到哪里去；<br />而`git push origin master`指定仓库和分支，就不会报错。 |                                                              |
|                                                              |                                                              |                                                              |
| `git push origin release --force`                            | 强制推送，内容起到置换效果，慎用                             | 自证                                                         |
|                                                              |                                                              |                                                              |
| `git push origin --all`                                      | 将本地`所有分支`都推送给`特定的远程仓库`                     |                                                              |
| `git push origin --tags`                                     | 当使用`--all`选项推送所有本地分支时，<br />`tags`并不会被自动推送到远程仓库。<br />故使用`--tags`选项来向远程仓库推送所有`本地tags`. |                                                              |

### **场景1：一套向 中心仓库 发布 本地仓库变更 的标准流程**

| **命令**                                                     | **作用**                                                     | **延展阅读**                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------------------------------------------------- |
| 1. `git checkout dev` <br />2. `git fetch origin main` <br />3. `git rebase -i origin/main` <br />4. # Squash commits, fix up commit messages etc. <br />5. `git push origin dev` | 1. 切到`本地dev分支`;<br />2. 通过`git fetch`同步`中心仓库main分支`在本地的副本，以确保本地副本是最新的;<br />3. 通过`git rebase`操作将分支的修改`变基`到`远程main分支`的`提交历史之上`;<br />(`-i` 表可交互的`rebase`操作，<br />同时也是在分享给其他团队成员之前，清理本地commit记录的好机会)<br />5. `git push`命令将`本地dev分支`发送`中心仓库`；<br />(由于已确保本地的`main`分支是最新版本的，因此`push`操作是能够快速前进的。此时git不会阻止`push`操作) | **[链接](https://www.jianshu.com/p/740b219c6546)** |

### **场景2：--amend  提交**

| 命令                                                         | 作用                                                         | 延展阅读                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--------------------------------------------- |
| 1. make changes to a repo and git add <br /><br />2. `git commit --amend` <br /><br />3. update the existing commit message <br /><br />4. push<br />方式1：`git push --force origin main`<br />方式2：`git push -f` | amend提交通常会`修正并更新commit message`，`或者增加新的修改`<br /><br />一旦一次commit被修正之后，`git push`会 直接失败，<br />因为Git认为修正之后的`commit`与`远程仓库的commit`发生了偏离。<br /><br />修正之后的`commit`需要使用`--force`选项才能推送到远程仓库 | [链接](https://www.jianshu.com/p/740b219c6546) |

### **场景3：`本地分支`和`远程分支`恰巧重名时 如何push?**

| **命令**                                                     | **作用**                                                     | **延展阅读** |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------- |
| 1. 本地新建`test4`分支，远程仓库没有重名分支<br /><br />2. `git checkout -b test4`<br />3. `date >> 1.txt && git add . && git commit -m "msg"`<br />4. `git push origin test4` | 1. 代码会被成功 `push` 到远程仓库<br />2. `.git/config`配置文件未追加关联关系 `[branch "test4"]`，故此分支依旧不可以直接执行`git push`<br />3. `.git\refs\remotes\origin` 里会追加文件 `test4` | 自证         |
|                                                              |                                                              |              |
| 1. 远程仓库有`test6`分支，本地有`test6`，他们没有关联关系，恰巧重名时<br /><br />2. `git checkout test6`<br />3. `date >> 1.txt && git add . && git commit -m "msg"`<br />4. `git push origin test4` | 1. 满足`fast-forward`则可以合入<br />2. 不满足`fast-forward`则被报错拦截<br />3. `.git/config`配置文件未追加关联关系 `[branch "test4"]`，故此分支依旧不可以直接执行`git push`<br />4. `.git\refs\remotes\origin` 里会追加文件 `test4` | **自证**     |

# **10. log**

## **10.1 查看log**

| 命令                       | 作用                                                         | 延展阅读                                              |
| :------------------------- | :----------------------------------------------------------- | :---------------------------------------------------- |
| `git log`                  | 输出 commit hsitory with `commit detail`                     |                                                       |
| `git reflog`               | 输出 `HEAD ref 的 reflog`                                    | [链接](https://www.cnblogs.com/onmpw/p/15748110.html) |
|                            |                                                              |                                                       |
| `git log --oneline`        | `--oneline`选项会把提交信息压缩输出在单行。<br />默认情况下，只显示commit id和commit message的第一行内容。<br /><br />$ git log --oneline<br/>8302eb4 (HEAD -> release3, origin/release2) dd<br/>5c50761 (origin/release1, main) Create 2.txt<br/>2b200df first commit<br/>fe5e47e Initial commit |                                                       |
| `git log --pretty=oneline` | $ git log --pretty=oneline<br/>8302eb4c5acd6cdc9417334b7ecd27dcc53acdcd (HEAD -> release3, origin/release2) dd<br/>5c50761f7ec0516668e2e870a29ae3747f0c840c (origin/release1, , main) Create 2.txt<br/>2b200df652b8902deab761746174df306fb54fab first commit<br/>fe5e47ec47e7c4fe300fa65dd7b37c29ddca2251 Initial commit |                                                       |
|                            |                                                              |                                                       |
| `git log --decorate`       | `--decorate`选项会让`git log`命令输出的同时显示关联引用（比如分支，tag之类的信息）<br />但是感觉--oneline也有，这个命令感觉被架空了 | [参考](https://juejin.cn/post/7090733619513622558)    |

## **10.2 查看graph**

| 命令                                         | 作用                                               |
| :------------------------------------------- | :------------------------------------------------- |
| `git log --graph`                            | 输出点线图 + commit信息                            |
| **`git log --graph --oneline`**              | 更言简意赅<br />                                   |
| `git log --graph --all`                      | 挖呀挖，挖出以前的记录                             |
| **`git log --graph --all --oneline`**        | 更言简意赅                                         |
| `git log --graph --all --oneline --decorate` | 和**`git log --graph --all --oneline`** 感觉没区别 |

# **11. Tag**

## **11.1 创建tag**

| **命令**                                        | **作用**                                       |
| :---------------------------------------------- | :--------------------------------------------- |
| **轻量标签**                                    |                                                |
| `git tag v1.0`                                  | 基于本地当前分支最新commit创建`tag v1.0`       |
| `git tag v.0325 125a1d1`                        | 给`指定commit 125a1d`打标签                    |
|                                                 |                                                |
| **附注标签**                                    |                                                |
| `git tag -a v.0329 -m "给标签添加说明" 125a1d1` | 基于指定commit创建标签并`添加说明`             |
| `git tag -a v.0329 -m "给标签添加说明" HEAD`    | 基于本地当前分支最新commit创建标签并`添加说明` |
| `git tag v2.0 -m "给标签添加说明"`              | 基于本地当前分支最新commit创建标签并`添加说明` |

## **11.2 查看tag**

| **命令**                      | **作用**                    |
| :---------------------------- | :-------------------------- |
| `git show v.0326`             | 查看`本地指定tag`的详细信息 |
| `git tag` / `git tag -l`      | 查看`本地所有tag`           |
| `git ls-remote --tags origin` | 查看`远程所有tag`           |

## **11.3 删除tag**

| 命令                                                         | 作用                 |
| :----------------------------------------------------------- | :------------------- |
| `git tag -d v.0325`                                          | 删除`本地tag v.0325` |
| 1. `git tag -d v.0325`<br />2. `git push origin :refs/tags/v.0325` | 删除`远程tag v.0325` |

## **11.4 推送tag**

| 命令                     | 作用                                |
| :----------------------- | :---------------------------------- |
| `git push origin v1.0`   | 推送`本地tag v1.0`到远程仓库        |
| `git push origin --tags` | 推送`全部未推送的本地tag`到远程仓库 |

# **12. 远程仓库别名**

## **12.1 查看远程仓库名称**

| **命令**        | **作用**                                                     |
| :-------------- | :----------------------------------------------------------- |
| `git remote`    | 查看关联的远程分支<br /><br />$ git remote<br/>origin        |
| `git remote -v` | 查看本地仓库关联的远程仓库信息<br /><br />$ git remote -v<br/>origin  git@github.com:kaku/reading-note-tutorails.git (fetch)<br/>origin  git@github.com:kaku/reading-note-tutorails.git (push) |

## **12.2 远程仓库别名增删改**

| **命令**                                                     | **作用**                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `git remote add github git@github.com:toFrankie/repo-demo.git` | 添加`远程仓库别名`为`github`                                 |
|                                                              |                                                              |
| `git remote rm github`                                       | 删除`远程仓库别名` `github` <br />只会移除本地仓库与远程仓库的关联，不会删除远程仓库的代码 |
|                                                              |                                                              |
| `git remote rename github gitlab`                            | 重命名远程仓库别名，从`github`改为`gitlab`                   |
| `git remote set-url github git@github.com:toXXXX/repo-demo.git` | 给当前别名 `github` 指定一个新的`远程仓库`                   |

## **12.3 设置远程仓库默认分支**

| **命令**                            | **作用**                                                  |
| :---------------------------------- | :-------------------------------------------------------- |
| `git remote set-head origin -a`     | 将 `origin/HEAD` 设为远程仓库的默认分支（`-a 即 --auto`） |
| `git remote set-head origin master` | 将 `origin/HEAD` 设为`远程分支master`                     |
| `git remote set-head origin -d`     | 删除`origin/HEAD`                                         |

# **13. 设置**

## **13.1 基础设置**

| **命令**                    | **作用**              |
| :-------------------------- | :-------------------- |
| `where git`                 | 查询 git 安装路径     |
|                             |                       |
| `git --help`                | 帮助文档              |
| `git add -help`             | 查看 add 命令帮助文档 |
|                             |                       |
| `git cat-file -p 8ds88f`    | 查看`git的对象的内容` |
|                             |                       |
| `locale`                    | 查看编码设置          |
| `export LC_ALL=en_US.UTF-8` | 设置编码属性          |
|                             |                       |
| `clear`                     | 清屏                  |

# **14. config**

## **14.1 查看 config**

| **命令**                                                     | **作用**                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `git config --local -l`                                      | 查看`仓库级别`git 配置信息                                   |
| `git config --global -l`                                     | 查看`全局级别`git 配置信息                                   |
| `git config --system -l`                                     | 查看`系统级别`git 配置信息                                   |
|                                                              |                                                              |
| `git config -l`                                              | 查看`所有级别`配置信息                                       |
|                                                              |                                                              |
| `git config --local --list --show-origin`<br />`git config --local -l --show-origin` | 查看`仓库级别` git 配置信息，并打印配置文件本地路径<br />最高优先级 |
| `git config --global --list --show-origin`<br />`git config --global -l --show-origin` | 查看`全局级别` git 配置信息，并打印配置文件本地路径<br />中间优先级 |
| `git config --system --list --show-origin`<br />`git config --system -l --show-origin` | 查看`系统级别` git 配置信息，并打印配置文件本地路径<br />最低优先级 |
|                                                              |                                                              |
| `git config -l | grep core`                                  | 正则匹配查询配置信息                                         |

## **14.2 编辑 config**

| 命令                                            | 作用                                                         | **延展阅读**                                                 |
| :---------------------------------------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| `git config --local -e`                         | 编辑`.git/config`文件，会自动调notepad++                     |                                                              |
|                                                 |                                                              |                                                              |
| `git config --global user.email "xxxxx@qq.com"` |                                                              |                                                              |
| `git config --global user.name "xxxxx"`         |                                                              |                                                              |
|                                                 |                                                              |                                                              |
| `git config --global --add user.age 10`         | 增加                                                         |                                                              |
| `git config --global --unset user.age`          | 删除                                                         |                                                              |
|                                                 |                                                              |                                                              |
| `git config --global alia.st status`            | 给命令配置`别名`，比如用st代替status                         |                                                              |
|                                                 |                                                              |                                                              |
| `git config –global core.autocrlf true`         | git在`push`时 `crlf → lf`，`pull`时 `lf → crlf` ；<br />本地`crlf`，仓库`lf`<br />(默认值)； |                                                              |
| `git config –global core.autocrlf false`        | git在 `push` 和 `pull` 的时候`不转换`；                      |                                                              |
| `git config –global core.autocrlf input`        | git在`push`时`crlf → lf`，pull时`不转换`；<br />本地`lf`，仓库`lf`；<br />(为了消除失误引入的`CRLF`) |                                                              |
|                                                 |                                                              |                                                              |
| `git config --global core.quotepath false`      | 修正 `git bash`中 中文显示为数字                             | [链接](https://blog.csdn.net/zhujiangtaotaise/article/details/74424157) |
|                                                 |                                                              |                                                              |
| `git config --global push.default simple`       | 不带任何参数的git push， 默认只推送当前分支，这叫做simple方式 |                                                              |
| `git config --global push.default matching`     | 还有一种叫做matching方式，会推送所有有对应的远程分支的本地分支 |                                                              |

# **15. 命令集**

## **场景1. 构造1个文件的10个commit**

| **命令**                                                     | **作用** |
| :----------------------------------------------------------- | :------- |
| **`for i in {1..10}; do date >> 66.txt && git add . && git commit -sm "update"; done`** | **自证** |

## **场景2. 构造10个文件**

| **命令**                                               | **作用** |
| :----------------------------------------------------- | :------- |
| **`for i in {1..10}; do date >> "file_$i.log"; done`** | **自证** |
