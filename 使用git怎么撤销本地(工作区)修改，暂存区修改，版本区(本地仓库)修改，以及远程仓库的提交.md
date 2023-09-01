# 使用git怎么撤销本地(工作区)修改，暂存区修改，版本区(本地仓库)修改，以及远程仓库的提交

大家在做项目的时候经常会使用git，但同时也会遇到很多问题，比如刚修改后得代码突然后悔了，这时怎么办呢

## 1、未使用`git add` 的时候----在工作区

新命令: git restore <file>

```#
git checkout -- filepathname //放弃修改某个文件
例如： git checkout -- readme.md
git checkout .  //放弃所有修改的文件

git restore . //放弃所有修改的文件

```

## 2、已经使用`git add` 的时候----在暂存区

新命令: git restore --stage <file>

```#
git reset HEAD filepathname //恢复某个文件到工作区
例如： git reset HEAD readme.md
git reset HEAD . //恢复所有文件到工作区
git  reset  //恢复所有文件到工作区

注意：这里只是恢复到了工作区，如果想放弃修改的代码还需要执行步骤1（工作区）中的操作

```

## 3.修改本地仓库

## 3、已经使用`git commit`提交的了代码----在版本区(本地仓库)

```#
1、如果你想全部撤回并回到远程仓库最新的状态(不保存代码修改)
	1、git reset --hard HEAD^ //回退上一次commit的状态
		//或git reset --hard commit_id //回退某个版本+id号就行
	2、git pull //拉取一下远程最新的
2、如果你想拉回工作区并保存修改。只撤销 commit 和 add(保存代码修改)
		git reset --mixed HEAD^
		或
		git reset HEAD^
3、如果你想撤销commit 但是不撤销 add(保存代码修改)
		git reset --soft HEAD^ //只撤销了git commit ， 修改后的代码还在暂存区
		
4 、终极版，由于你太懒，不管是暂存区，版本区，你只想撤销修改并回到远程最新的版本(不保存代码修改)
 	1 、git fetch --all
 	2、git reset --hard origin/master //git reset --hard origin/远程分支名
	注意：这里只在暂存区和版本区哦

```

## 4、如果你`git push` 到了远程分支，这时候你后悔了怎么办

```#
胆小勿试，搞之前记得做备份
第一种
回滚远程分支的最近一次提交
git revert HEAD
git push origin 分支名
例如：我刚在master分支提交了一次
   git revert HEAD
   git push origin master
慎用啊~~~这种方式会在远程生成一个版本号

第二种
git reset --hard HEAD^ //回退上一个版本
或
git reset --hard commit_id //回退到某个版本 id就是你的版本号
git push origin HEAD --force //强制推送到远程，可能会受到保护

```

**自己总结的，如果有哪些问题或者不对的地方，也希望大家能给出指导意见。谢谢！**

