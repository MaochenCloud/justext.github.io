## Git基本概念
### 项目在git中的不同工作区
  ![git-workflow]({{"/assets/git-learn/gitworkflow.png" | absolute_url }})   
* **工作区（working）**: 当 git clone 一个项目到本地，相当于在本地克隆了项目的一个副本。工作区是对项目的某个版本独立提取出来的内容。这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供使用或修改。
* **暂存区（staging）**: 暂存区是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。有时候也被称作 `‘索引’'，不过一般说法还是叫暂存区。
* **本地仓库（local）**: 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 本地仓库。
* **远程仓库（remote）**: 以上几个工作区都是在本地。为了让别人可以看到你的修改，需要将更新推送到远程仓库.

### .git目录
*COMMIT_EDITMSG*  **HEAD**  *ORIG_HEAD*   **config**  *description* ... *logs*  *objects*  **refs**
* **HEAD**: refs/heads/master, 当前的分支在master.
* **config**: core和user配置信息.  
* **refs**: 目录里面存放branch信息和commit指针.

### commit、tree、blob
每个commit都有一个hash值，commit里面包含一个tree，tree中的blob对应着文件.
### 分离头指针
HEAD不再指向分支，而是直接指向某个commit，git可能会将改动删除.  
```shell
$ git checkout  <commit-hash>
Note: checking out 'f20a7ad'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at f20a7ad add readme file~

$ git log
commit f20a7ad7f08a2732bb6a53eb4af96bc159d4e132 (HEAD)
Author: Thomas <Thomas.intel.com>
Date:   Thu May 19 13:42:33 2022 +0000

    add readme file

commit 7e49d01d1f0817b1363ae5d83fcc5d5856d78341
Author: Thomas <Thomas.intel.com>
Date:   Thu May 19 13:28:53 2022 +0000

    add 1.c
```
     
## Git命令
### git的最小配置
```shell
git config --global user.name "Thomas"
git config --global user.email "Thomas@intel.com "
git config --global --list
```
### 创建第一个工作仓库
```shell
git init hello-world && cd hello-world
git config --local user.name "Thomas"
git config --local user.email "Thomas.intel.com"
```
### 增删查分支
```shell
# 列出所有的分支
git branch -av

# 基于当前分支创建新分支
git branch <new-branch>

# 强制删除本地分支，将会丢失未合并的修改
git branch -D <branch>
```
### 切换分支
```shell
# 切换分支
git checkout <branch>

# 创建并切换到新分支,本地branch与远端branch保持一致
git checkout -b <branch> origin remote/<branch>
```
### 添加修改到暂存
```shell
# 把指定文件添加到暂存区
git add <file>

# 把当前所有修改添加到暂存区
git add .
```
### 提交修改到本地仓库
```shell
# 提交本地的所有修改
git commit -a

# 附加消息提交
git commit -m 'commit message'
```
### 打包应用
```shell
# 打包HEAD的第一个commit,生成0001-commit_message>.patch
git format-patch -1 / git format-patch HEAD~1

# 打包HEAD的第一个第二个commit,生成0001-commit_message>.patch, 0002-commit_message>.patch
git format-patch -2 / git format-patch HEAD~2
```
### 显示工作路径下已修改的文件
```shell
git status
```
### 文件重命名
```shell
mv readme readme.md
git add readme.md
git rm readme
git commit -m "mv readme to readme.md"
# OR
git mv readme  readme.md
git commit -am "mv readme to readme.md"
```
### 文件删除
```shell
git rm <file>
```
### 查看git版本历史
```shell
# 近期4条commit
git log -n4

# 所有branch图形化显示
git log --all --graph 
```
### 存储
有时，需要在同一个项目的不同分支上工作。当需要切换分支时，偏偏本地的工作还没有完成，此时，提交修改显得不严谨，但是不提交代码又无法切换分支。这时，可以使用 git stash 将本地的修改内容作为草稿储藏起来.
```shell
# 将修改作为当前分支的草稿保存
git stash

# 查看草稿列表
git stash list

# 读取存储，弹出栈区，不保留堆栈，且每个分支共享一个堆栈
$ git stash pop
```
### 版本比较
```shell
# HEAD与上级commit比较、HEAD与上上级commit比较
git diff HEAD HEAD~1
git diff HEAD HEAD~2

# 不同commit比较
git diff <commit-hash1> <commit-hash2>

# 工作区和暂存区比较
git diff

# 暂存区和版本区比较
git diff --cached

# 工作区和版本区比较
git diff HEAD
```
### 当前目录中查找文本内容
```shell
git grep "Hello"
```
### 撤销修改
```shell
# 暂存区和版本区保持一致,清空暂存区
git reset HEAD

# 暂存区的部分文件的取消修改
git reset HEAD 1.c

# 将HEAD重置到指定的版本，并抛弃该版本之后的所有修改
git reset --hard <commit-hash>

# 放弃工作目录下的所有修改
git reset --hard HEAD

# 用远端分支强制覆盖本地分支
git reset --hard <remote/branch>

# 放弃某个文件的所有本地修改
git checkout HEAD <file>

# 工作区恢复到暂存区
git checkout <file>
```
### 删除远端指定版本
```shell
git reset --hard <commit-hash>
git push -f
```
### 从远端更新本地
```shell
# 下载远程端版本，但不合并到HEAD中
git fetch <remote>

# 将远程端版本合并到本地版本中
git pull origin master
```
### 从本地推送远端
```shell
# 将本地版本推送到远程端
git push -u origin <branch> 

# 删除远程端分支
git push origin --delete <branch>
```
## Git常见的场景
### 修改最新的commit的message
```shell
git commit --amend
```
### 修改过去的commit的message
~~~
======================================================================
Target: 修改HEAD~1的commit message.
Historical commit: 
  commit b72a38cbe417766d4e4e793e9d2b72a64b6a8982 (HEAD -> testMchen)
    feat: Add 3.c
  commit de24974c93074f06231174eb5c0afd076416d743
    move readme to readme.md
  commit f20a7ad7f08a2732bb6a53eb4af96bc159d4e132
    add readme file
======================================================================

# 修改HEAD~1的message,需要进入HEAD~2的进行修改，不直接修改HEAD~1, hash值会改变.
$ git rebase -i f20a7ad7 
# 选择 r, reword = use commit, but edit the commit message
# pick 1614c10 move readme to readme.md --> r 1614c10 move readme to readme.md
~~~
### 合并连续的commit
~~~
======================================================================
Target: 合并HEAD~1,HAED~2的commit message.
Historical commit:
  commit cc37e3adc075a42f1bb83b693436998131ee37a9 (HEAD -> testMchen)
    feat: add 3.c  
  commit b727c6db949da0c97f47b264e70cab7435d1d88f
    feat: move readme to readme.md
  commit 66410b885cc6cb8f1f3a37c8ffc8c7b4d5339e82
    feat: add readme file
  commit 7e49d01d1f0817b1363ae5d83fcc5d5856d78341
    feat: add 1.c
======================================================================

$ git rebase  -i 7e49d01d
# 选择 s, squash = use commit, gbut meld into previous commit
~~~

### 本地项目上传github
~~~
# 生成公私钥
$ ssh-keygen -t rsa -C "mao.chen@intel.com" 
# copy the id_key.pub to the github settings 
# New repository -> add owner,repository name, public -> create repository 
$ git remote add origin https://github.com/MaochenCloud/git_learning.git 
$ git push -u origin main
~~~
### 多人维护相同分支的不同文件
```shell
$ git pull origin
# git checkout -b (本地名(一般与线上分枝名一致)) origin／线上分支名
$ git checkout  -b <branch> origin/<branch> 
$ git add <file>
$ git commit -s -m "commit_message"
$ git push -u origin <branch>
```
### 多人维护相同文件的不同区域
~~~
======================================================================
Target: A,B共同维护readme
Background: A完成提交,B在没有A的最新提交commit，实施提交自己的commit
Errors: 
$ git push  -u origin  add_readme
To https://github.com/MaochenCloud/git_learning.git
 ! [rejected]        add_readme -> add_readme (fetch first)
error: failed to push some refs to 'https://github.com/MaochenCloud/git_learning.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
======================================================================

$ git pull origin
$ git push  -u origin  add_readme
~~~
### 多人维护相同文件的相同区域
~~~
======================================================================
Target: A,B共同维护readme
Background: A完成提交,B在没有A的最新提交commit，实施提交自己的commit
Errors-0: 
$ git push -u origin  add_readme
To https://github.com/MaochenCloud/git_learning.git
 ! [rejected]        add_readme -> add_readme (non-fast-forward)
error: failed to push some refs to 'https://github.com/MaochenCloud/git_learning.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
Errors-1:
$ git pull origin
Auto-merging readme.md
CONFLICT (content): Merge conflict in readme.md
Automatic merge failed; fix conflicts and then commit the result.

5
<<<<<<< HEAD
b 5.bkp
=======
A 5.bkp
>>>>>>> 0a14bc7b3860827f49ff0d278230f9de4d65c701
6
======================================================================

$ git pull origin
# B的readme.md有冲突,解决冲突，保留或者合并
$ git add readme.md
$ git commit -s -m"resloved conflict by hand with 5"
$ git push -u origin add_readme
~~~
### 更名文件且他人修改文件
~~~
======================================================================
Background: A已经提交readme.md的变更readme，B没有A的最新commit.
$ git push  -u origin  add_readme
To https://github.com/MaochenCloud/git_learning.git
 ! [rejected]        add_readme -> add_readme (fetch first)
error: failed to push some refs to 'https://github.com/MaochenCloud/git_learning.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
======================================================================
$ git pull origin
$ git push  -u origin  add_readme
~~~
### 多人协作时多分支集成
~~~
======================================================================
Target: 向别人分支branch_a上，提交自己分支branch_b
======================================================================
$ git checkout branch_a
$ git pull 
$ git checkout -b branch_b origin branch_b 
$ git rebase branch_a
# 如有conflict就fix
$ git add <file>
# git rebase --abort是舍弃之前改动回到最初的状态，git rebase --continue会一条条过conflict,只要git add保留状态
$ git rebase --continue
$ git push -u origin -f branch_b 
~~~
### 打包commit并且apply
~~~
======================================================================
Target: 将当前的patch，应用到HEAD~2版本之上
Background: 
  commit cac9c8349b43e85537a770c4693e0fd3191e422c (HEAD -> testMchen)
    rm 6~10.c
  commit a921d81a9806a29f54e6b323e2c8be83b1677571
    add 10.c
  commit c7f11a34056962e1f5bd83a70ec9a77dd4e956ba
    add 7.c
  commit 7593b17ea2069a93911777dd75851de16ac0ae17
    add 5.c
======================================================================

# 将最近的两次commit打包，生成两个patch文件 0001-add-10.c.patch  0002-rm-6-10.c.patch  
$ git format-patch HEAD~2
$ git reset --hard HEAD~2
$ git am 0002-rm-6-10.c.patch
~~~
### 慎用git push -f origin
~~~
======================================================================
Experiment:
$ git reset --hard e244c088
$ git commit -s -m "delete some commit"
$ git push origin  -u add_readme
To https://github.com/MaochenCloud/git_learning.git
 ! [rejected]        add_readme -> add_readme (non-fast-forward)
error: failed to push some refs to 'https://github.com/MaochenCloud/git_learning.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
$ git push -f origin -u add_readme
======================================================================

# git push -f  origin 会把之前的commit全部删除
~~~
### 集成分支上不做rebase
> 会导致分支上的hash值改变，其他人的commit的是老的hash值,无法跟新.
> 如要修改以前的commits信息,可以提交新的commit，而不是rebase变基,当然自己创建的维护的分支可以变基.






