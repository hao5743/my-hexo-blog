---
title: git status可能的情况
date: 2016-12-29 17:29:54
tags: [git]
---

# git status可能的情况

工作中我们使用git作为版本管理工具，它的确是一个强大的工具。易于学习和使用，但是要学习的面面俱到也很难，就`git status`就可能会出现n多种的情况。本文记录了`git status`可能出现的不同状态信息、状态说明、处理办法。注意，列举的情况并不全面，只是列举了工作中遇到的一些常见情况，同时遇到新状态，还会随时增加。

**git仓库中文件的状态**
git仓库中所有文件可分为两种状态**已跟踪**和**未跟踪**。已跟踪的文件被git纳入了版本控制，git会记录它的每次更新状态。
状态生命周期有四种：untracked, unmodified, modified, staged

**git status可能的情况：**

##  1. Your branch is ahead of 'origin/master' by 1 commit.

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
cochona@cochona:~/gitosis-admin/keydir$ 
```
此时说明当前目录中所有的已跟踪文件，都已经和远程保持同步，并且没有修改。刚从远程克隆的git仓库，都是这个状态。
	
## 2. nothing added to commit but untracked files present

```
$ git status
On branch master
Initial commit
Untracked files:
	(use "git add <file>..." to include in what will be committed)
a.text
nothing added to commit but untracked files present (use "git add" to track)
```
	
表示此时目录下，有新添加的未被跟踪的文件，如果我们需要将此文件提交到仓库，可以执行git add
	
## 3. Changes to be committed: new file
	
```
$ git status
On branch master
Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
   new file:   README
```
	
在“Changes to be committed”这行下面的都是已暂存的状态，此时可以执行git commit


## 4. Changes to be committed: modified
	
```
cochona@cochona:~/gitosis-admin$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
	
	modified:   gitosis.conf
```
有修改的文件，此时可以选择直接`git commit -m "info"`	
## 5. Changes not staged for commit: modified
	
```
cochona@cochona:~/gitosis-admin$ git commit -m "add shaoc@pub to admin-group"
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
	modified:   gitosis.conf
	
no changes added to commit
```
	
此时如果要保存修改，需要先`git add gitosis.conf`，然后`git commit -m ""`,然后`git push`
	

