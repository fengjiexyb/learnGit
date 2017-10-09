本文是[廖雪峰](http://www.liaoxuefeng.com)git教程的笔记，添加一些自己在网上的学习以及自己做的git架构图。如有错误，请大家轻拍。

架构图
 ![git架构图](https://raw.githubusercontent.com/fengjiexyb/learnGit/master/git.jpg)

初始化
>
* git init 初始化本地库

修改和提交
>
* ```git add filename``` 添加修改的文件到缓存区
 `git add -A` 添加所有修改到缓存区，`A`大写
 `git add .` 添加所有修改到缓存区
* `git commit -m "info"`    暂存区(stage)的修改提交到本地库（master）,引号内必须有文字，否则不会提交
 * `git commit --amend` 修改最后一次提交
* `git rm filename` 删除版本库中的文件(可以同时删除工作区和缓存区）所以需要commit
* `git mv oldfilename newfilename` 修改文件名，工作区和缓存区一起修改，所以需要commit

忽略
>
* 忽略特殊文件(不提交）
> 在Git工作区的根目录下创建一个特殊的```.gitignore```文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
[模板](https://github.com/github/gitignore)
```.gitignore```也提交到Git
**注意**windows下要用记事本另存为才可以，否则要求必须输入文件名。
* `git check-ignore -v App.class` 检查```App.class```文件和哪一条```.gitignore```冲突
* `git add -f App.class` 即使```.gitignore```文件中有```App.class```也要提交

查看
>
* `git status` 展示哪些文件（包括工作区、缓存区和本地库）增加、修改（不显示具体内容，具体内容使用diff）、删除
* `git diff` 工作区(work dict)和暂存区(stage)的比较，不显示增加删除，只显示修改文件
    * `git diff <source_branch> <target_branch>` 两个分支的比较（合并前）
* `git log` 显示提交历史。```-p```可以显示所有的具体修改，否则只显示日志信息
    * `git log --graph` 查看提交版本图
* ```git reflog```查看命令历史，以便确定要回到未来的哪个版本。


撤销
>
* `git reset --hard HEAD^` 用本地库的上一个版本覆盖工作区
 - 在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。
 - 使用`--hard`参数表示本地库回退到上一版本，缓存区、工作区同样。
   使用`--soft`参数表示只有本地库回退，缓存区、工作区不变
   使用`--mixed`参数表示回退本地库和缓存区，工作区不变。这也是默认模式。
    - windows下使用这个命令 ```git reset --hard "HEAD^" ```
    *  `git reset -- HEAD filename`
   用本地库覆盖缓存区（即未使用commit时使用），注意和上面的区别。
*  `git checkout -- filename `
    用暂存区覆盖工作区（即未使用add时使用）
* `git revert <commit>` 撤销指定的提交
* git revert 和 git reset的区别
1. git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。
2. 在回滚这一操作上看，效果差不多。但是在日后继续merge以前的老版本时有区别。因为git revert是用一次逆向的commit“中和”之前的提交，因此日后合并老的branch时，导致这部分改变不会再次出现，但是git reset是之间把某些commit在某个branch上删除，因而和老的branch再次merge时，这些被回滚的commit应该还会被引入。
3. git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要被revert的内容。

分支
>
*  `git merge dev` 合并dev分支到当前分支。冲突时修改文件之后重新add、commit即可。不用再merge
    * `git merge --no-ff -m "merge with no-ff" dev` 使用非```Fast forward```方式来合并分支，这种方式在删除分支之后可以使用```git log --graph```查看分支记录。```-m```表示合并说明。
**注意**如果合并时有冲突，无论哪种方式都会保存记录的。
* `git branch` 查看所有分支，当前分支前有
    * `git branch dev` 创建分支dev，创建分支时父分支必须提交。
    * `git branch -d dev` 删除dev分支 ```-D```表示强制删除（分支没有合并时）。
    * `git branch --set-upstream dev origin/dev`                设置dev（本地）和origin/dev（远程）的链接
* `git checkout dev` 切换到dev分支
    * `git checkout -b dev` 创建并且换到dev分支
    * `git checkout dev origin/dev` 将创建一个分支（dev），该分支和远程分支对应（origin/dev）（对应的意思是，这时没本地分支，如果应经有了，那么使用下面的```git branch --set-upstream dev origin/dev```）

远程
>
* `ssh-keygen -t rsa -C "youremail@example.com"`（这个实在git bash中输入的，不是工作区的命令窗口） 建立ssh，后面要输入文件路径，我只能用默认路径，改不了。但是这个路径其实是可以在任意位置的（不一定非要在项目路径下）
* `git clone`
>1. 最简单直接的命令
`git clone xxx.git`
>2. 如果想clone到指定目录
`git clone xxx.git "指定目录"`
>3. clone时创建新的分支替代默认Origin HEAD（master）
`git clone -b [new_branch_name]  xxx.git`
* `git remote add origin git@github.com:fengjiexyb/learngit.git`
这个命令实际是将本地库和`learngit`库相连（也可以用rm删除链接）。
** 注意：** 一定要写左斜杠
`origin`可以自己起名，这个是默认名
`fengjiexyb`：github用户名
`learngit`：项目名（repository）
* `git pull origin master` 从网络库下载资源，并快速合并。```master```是本地分支名
* `git fetch origin master`
从网络库下载资源，但不会合并。```master```是本地分支名
* `git push [-u] origin master` 上传资源到网络库，并快速合并（只有第一次需要使用-u）。```master```是本地分支名
* `git push  origin: master` 删除远程分支master
* `git push origin v1.0` 推送v1.0到远程
* `git push origin --tags` 推送所有未推送的标签到远程库
* `git push origin :refs/tags/v0.9` 删除远程库的v0.9标签（要先删除本地的v0.9）
* `git remote -v` 查看远程版本库信息
* `git remote show origin`查看远程版本库origin信息


储存
>
* `git stash` 保存工作区镜像，不需要提交
* `git stash list` 查看所有镜像
* `git stash apply` 恢复镜像，`stash`内容并不删除
* `git stash drop` 删除镜像
* `git stash pop` 恢复并删除镜像
* `git stash apply stash@{0}` 恢复镜像stash@{0}

标签
>
* `git tag` 查看标签
`git tag v1.0` 设置当前分支的标签名为v1.0（标签实际对应的是commit，参见下条）
`git tag v0.9 6224937` 设置commit id为6224937 的分支的标签为v0.9
`git tag -a v0.1 -m "version 0.1 released" 3628164` 设置标签的同时设置说明（-m）
`git tag -d v0.1` 删除标签v0.1
* `git show v0.9` 显示v0.9 的commit


