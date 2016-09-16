##Git 笔记

###config

	git config --global user.name
	git config --global user.email
设置分为三个级别：system，global，local，像面向对象继承一样，设置由远至近

###init

	git init [repository name]
从已有项目或从头设置一个新仓库

###commit

	git status
	git add file
	git status
编辑文件，并将其存入暂存区

	git commit -m "commit describtion"
	git status
提交至代码库

###diff

	git diff
对工作树也就是文件系统本身所做的修改进行比较

	git diff --staged
工作树和暂存区进行比较

	git diff HEAD
工作树和上次提交进行比较

	git diff --color-words
	git diff --word-diff

	git diff --stat
配合各种参数进行差别词，差别文件比较

###log

	git log
	git log --online
	git log --stat
	git log --patch

	git log --graph --all --decorate --oneline
以时间先后生成提交记录，各种参数不相互冲突

###rm

	git rm file
从工作树及暂存区删除文件或如果先rm了文件

	git status
	git add -u .
	git commit -m "remove files"
遍历工作树，暂存区记录删除的文件

	git rm --cached file
	git status 
不再跟踪一个文件从却保留在工作树中
	
###mv

	git mv src dst
	git status
从工作树及暂存区移动文件，重命名和移动是一样的

	git status
	git rm src
	git add dst
如果先mv了文件

	git add -A .
遍历工作树，暂存区记录移动的文件

	git commit -m "change content and move"
	git log --stat -M --follow -- folder/file
使日志在移动中追踪文件， -M后可更改阈值

默认有一个50%的相似度阈值，认为这是一次移动，而不是删除和添加，从而进行跟踪。在合并中即使文件结构发生改变也不发生冲突原因也是阈值

###.gitignore

###branch

	git branch
	git branch branchname
	git branch -d/-D(如果没有合并) branchname
添加删除分支

	git checkout branchname
更改工作树至当前分支

###checkout

	git log
	git checkout 哈希值
	git checkout master
工作树更改为历史的一次提交

	git status
	git chekout -- file
清除掉未暂存内容

	git checkout -b branchname
创建并切换分支 

###merge	

	git checkout master
	git merge branchname
	git log -2
合并某一分支

	git status
	vim file
	git add file
	git commit 
当两个分支差异较小时，会发生冲突。要去除冲突，先打开冲突文件，` <<<<<<< HEAD `后表示的是当前分支的内容，之后由`========`分割，然后是想要合并的内容，之后是`>>>>>> branchname`表示冲突结束的位置。手动去除冲突，暂存，提交

	git merge --abort
当出现冲突，merge结果不想保留时，这会回退工作树，并清除暂存区

	git merge --squash branchname
	git status
本地文件内容与不使用该选项的合并结果相同，但是不提交、不移动HEAD，因此需要一条额外的commit命令。其效果相当于将another分支上的多个commit合并成一个，放在当前分支上，原来的commit历史则没有拿过来。

合并后可删除被合并分支

###remote

	git remote add origin(目的地名字) URL(目的地URL)
	git remote set-url origin target-url
设置与更改远程仓库

	git remote rm origin
删除远程仓库

	git remote -v
查看当前远程仓库

	git fetch origin
	git branch -r
下载远程仓库

	git checkout feature
	git pull origin
将下载并合并origin/feature中的内容至本地

	git push origin master
推送至远程

###reset

	git status
	git reset HEAD
清除暂存区，放回工作树中

reset有三种模式：mixed，soft，hard

	git reset --soft HEAD~5
	git status
	git commit -m "five new changes"
最新5条commit合为1个commit

	git reset --hard HEAD~5
丢弃掉最近5次提交

	git checkout 哈希值	file
	git status
回到一个特定文件的某次提交历史上
 
checkout更加关注在一个文件或目录的精度上

###reflog

	git reflog
打印出历史记录

	git reset --hard 哈希码
当前分支被强制切换到历史点

###rebase

	git checkout 特色分支
	git rebase master
从最新点加入变更，相比merge有更清晰的历史
