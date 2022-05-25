# Git基本概念
## 项目在git中的不同工作区
- 工作区（working）: 当 git clone 一个项目到本地，相当于在本地克隆了项目的一个副本。工作区是对项目的某个版本独立提取出来的内容。这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供使用或修改。
- 暂存区（staging）: 暂存区是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。有时候也被称作 `‘索引’'，不过一般说法还是叫暂存区。
- 本地仓库（local）: 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 本地仓库。
- 远程仓库（remote）: 以上几个工作区都是在本地。为了让别人可以看到你的修改，需要将更新推送到远程仓库。
![git-workflow](https://github.com/MaochenCloud/justext.github.io/blob/master/assets/git-learn/gitworkflow.png)
## .git目录
COMMIT_EDITMSG  HEAD  ORIG_HEAD  branches  config  description  hooks  index  info  logs  objects  refs
- HEAD: refs/heads/master, 当前的分支在master.
- config: core和user配置信息.  
- refs: 目录里面存放branch信息和commit指针。
## commit、tree、blob三者对象的关系
每个commit都有一个hash值，commit里面包含一个tree，tree中的blob对应着文件.
## 分离头指针
HEAD不再指向分支，而是直接指向某个commit，git可能会将改动删除.
```
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
     

# Git命令
## git的最小配置,配置user.name和user.email
```
git config --global user.name "Thomas"
git config --global user.email "Thomas@intel.com "
git config --global --list
```
## 创建第一个工作仓库
```
git init hello-world && cd hello-world
git config --local user.name "Thomas"
git config --local user.email "Thomas.intel.com"
```
## 增删查分支
```
# 列出所有的分支
git branch -av

# 基于当前分支创建新分支
git branch <new-branch>

# 强制删除本地分支，将会丢失未合并的修改
git branch -D <branch>
```
## 切换分支
```
# 切换分支
git checkout <branch>

# 创建并切换到新分支,本地branch与远端branch保持一致
git checkout -b <branch> origin remote/<branch>
```
## 添加修改到暂存
```
# 把指定文件添加到暂存区
git add 001.c

# 把当前所有修改添加到暂存区
git add .
```
## 提交修改到本地仓库
```
# 提交本地的所有修改
git commit -a

# 附加消息提交
git commit -m 'commit message'
```
## 显示工作路径下已修改的文件
```
git status
```
## 文件重命名
```
mv readme readme.md
git add readme.md
git rm readme
git commit -m "mv readme to readme.md"
# OR
git mv readme  readme.md
git commit -am "mv readme to readme.md"
```
## 文件删除
```
git rm 001.c
```
## 查看git版本历史
```
# 近期4条commit
git log -n4

# 所有branch图形化显示
git log --all --graph 
```
## 存储
有时，需要在同一个项目的不同分支上工作。当需要切换分支时，偏偏本地的工作还没有完成，此时，提交修改显得不严谨，但是不提交代码又无法切换分支。这时，可以使用 git stash 将本地的修改内容作为草稿储藏起来.
```
# 将修改作为当前分支的草稿保存
git stash

# 查看草稿列表
git stash list

# 读取存储，弹出栈区，不保留堆栈，且每个分支共享一个堆栈
$ git stash pop
```
## 版本比较
```
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
# 从当前目录的所有文件中查找文本内容
```
git grep "Hello"
```
## 撤销修改
```
# 暂存区和版本区保持一致,清空暂存区
git reset HEAD

# 暂存区的部分文件的取消修改
git reset HEAD 1.c

# 将HEAD重置到指定的版本，并抛弃该版本之后的所有修改
git reset --hard <commit-hash>

# 放弃工作目录下的所有修改
git reset --hard HEAD

# 用远端分支强制覆盖本地分支
$ git reset --hard <remote/branch>

# 放弃某个文件的所有本地修改
$ git checkout HEAD <file>

# 工作区恢复到暂存区
git checkout <file>
```
## 删除远端指定版本
```
git reset --hard <commit-hash>
git push -f
```
## 从远端更新本地
```
# 下载远程端版本，但不合并到HEAD中
git fetch <remote>

# 将远程端版本合并到本地版本中
git pull origin master
```
## 从本地推送远端
```
# 将本地版本推送到远程端
git push -u origin <branch> 

# 删除远程端分支
git push origin --delete <branch>
```

# Git常见的场景



























