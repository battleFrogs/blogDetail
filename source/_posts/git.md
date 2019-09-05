---
title: git常用命令
layout: post
tags:   
  - 常用命令  
---

Git是目前世界上最先进的分布式版本控制系统,可以有效、高速地处理从很小到非常大的项目版本管理。

<!--more-->


### git的安装

	git config --global user.name "Your Name"  
	git config --global user.email "email@example.com"

### git的初始化

	git init

### git的添加
	git add file

### git的提交
	git commit -m "xxxx"

### 查看仓库状态
	git status   
	git diff file  查看修改的内容  
	cat file 查看文件内容

### 版本的回退
	git log 显示最近的提交的日志  
	git show <commit_id>   查看对应的修改
	git log --pretty=oneline 显示简单的日志  
	git reflog 查看命令历史，回到未来  
	git reset --hard HEAD^ 回到上一个版本  
	git reset --hard 1321554  回到指定的版本
	git push -f  推送修改后的远程库的版本，强制性   
	
	- 当提交一个commit，HEAD会到一个新的记录中
    - commit以后,需要回退要指定前一个（HEAD~）版本，或者再log中指定前一条记录rest current branch  soft:保存当前记录，且到了上一条记录。mixed:以已经add的状态再上一条记录中呈现。hard：直接舍弃掉记录。到上一条记录。
    - add以后，回退，需要指定当前HEAD版本就行，或者再log中指定当前一条记录mixed

### 撤销修改

	git checkout -- file  撤销修改文件一种是工作区修改未提交，一种是暂存区修改未提交  
	git checkout . && git clean -df   放弃所有修改，新增，删除文件（ -n 显示 将要 删除的 文件 和  目录  -f 删除 文件，-df 删除 文件 和 目录）
	git reset HEAD file 已经提交到暂存区 ，然后在checkout操作  
	git reset --hard 1635132 已经提交到版本库中 ，只能回归版本号  然后  git push -f  推送修改后的远程库的版本，强制性
	git revert HEAD  //已经提交到版本库中,多加一次提交，撤销最近上一次提交
	

### 远程库
	git remote add origin git@server-name:path/repo-name.git  第一次提交需关联远程库的操作  
	git push -u origin master 第一次推送内容  
	git push origin master 推送最新的修改  
	git clone git@github.com:battleFrogs/xx.git  从远程库中拉取文件   


### 分支管理

	git branch 查看分支  
	git branch xx 创建分支  
	git checkout name 切换分支  
	git checkout -b name 创建并切换分支  
	git merge xx 合并到当前分支  
	git branch -d xx 删除分支  
	git branch -a xx 已经更新分支
	
	#stash@{1}表示第一个，stash@{num}:表示第num个
	#文件必须先add后，才能stash
	git stash 保存现场  
	git stash save "save message"  : 加以备注保存现场
	git stash list 查看stash了哪些存储
	git stash show ：显示做了哪些改动，默认show第一个存储,如果要显示其他存贮，后面加stash@{$num}，比如第二个 git stash show stash@{1}
	git stash apply 恢复但不删除stash    
	git stash drop   删除stash  
	git stash pop 恢复现场  
	git stash clear ：删除所有缓存的stash
	
	git branch -D name 强行删除分支  
	git branch --set-upstream dev origin/dev  指定本地dev分支与远程origin/dev分支的链接 
	git remote -v 查看远程库地址
	git push origin local_branch:remote_branch   推送本地分支到远程分支
	git branch -r -D origin/BranchName  删除本地的远程分支    
	git push origin -d BranchName  远程删除git服务器上的分支：
	git remote remove origin  有关联远程库的，先删除远程库关联
	git remote add origin https://dev.33.cn/wangdong/alioss-file.git  重新关联新的远程库
	
	git cherry-pick dev-3.0分支的某些commit-hash    # dev-3.0分支上的某些commit在release-2.1分支上重演
	（在新的分支上merge旧的分支某些内容  # git cherry-pick用法）
	

### tag标签
	git tag v1.0  标记标签
	git tag  查看标签  
	git tag v0.9 1651465  给之前的commit打tag  
	git tag -a tagname -m "blablabla..." 指定标签信息  
	git push origin tagname 推送本地tag  
	git push origin --tags  推送全部未推送的tag   
	git tag -d tagname  删除本地标签  
	git push origin :refs/tags/v0.9 To git@github.com:michaelliao/learngit.git[deleted]         v0.9   删除远程tag      
	git checkout -b branch_name tag_name  在tag中切换出分支


### 合并分支   

    git merge branchName  合并分支    
    git merge origin/banchName 合并远程分支   
    
    （修改某个功能时，频繁进行了git commit提交时，发现其实过多的提交信息没有必要时，可以尝试git rebase）
    git rebase branchName  合并分支
    git add .  解决完冲突1，然后执行添加缓存区
    git rebase --contine 继续合代码2
    git add .  解决完冲突2，然后执行添加缓存区
    git rebase --contine 继续合代码3
    
    git rebase -i HEAD~2  将这两个版本的提交内容进行操作
    git rebase --abort 终止rebase行动