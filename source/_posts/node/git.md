---
layout: _posts
title: git 使用指南
date: 2018-10-26 09:29:30
tags: pm2
categories:
- Node.js
description: 原文来自《廖雪峰 Git 教程》
---
## 一、简介
### [《廖雪峰 Git 教程》](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
### Git 是什么？
Git是目前世界上最先进的分布式版本控制系统。
## 二、命令
我们把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到(stage)暂存区;
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/0.jpeg)
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/1.jpeg)
一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的。
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/2.png)
当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/3.png)
你看，Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！
不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/4.png)
假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/5.png)
所以Git合并分支也很快！就改改指针，工作区内容也不变！
合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/6.png)
### 初始化一个 Git 仓库
```
git init
```
### 查看仓库当前状态
```
git status
```
### 添加修改至暂存区
```
git add file
```
### 查看文件修改内容
```
git diff readme.txt
```
### 添加修改至分支
```
git commit -m "xxx"
```
### 修改 commit message
```
git commit --amend

git commit --amend -m "xxx"
```
### 丢弃工作区的修改
```
git checkout -- file
// 就是让这个文件回到最近一次 git commit 或 git add 时的状态
```
### 查看 commit 历史记录
```
git log --graph --pretty=oneline --abbrev-commit
```
### 版本回退
```
// 回退至上N个版本
git reset --hard HEAD^
git reset --hard HEAD^^

// 回退至往上100个版本
git reset --hard HEAD~100

// 回退至某个版本
git reset --hard commit_id
```
### 版本回退后恢复
```
// 查看历史命令
git reflog
```
### 删除文件
```
git rm file

git commit -m "remove file"

// 撤销
git checkout -- file
```
### 把当前工作现场“储藏”起来
```
git stash
```
### 恢复现场后继续工作
```
git stash list

git stash apply
// 恢复后，stash内容并不删除，你需要用git stash drop来删除

git stash pop
// 恢复的同时把stash内容也删了
```
## github 仓库
### 把一个已有的本地仓库与之关联
```
git remote add origin git@github.com:VonJie/learngit.git

// 第一次 push 时，参数 -u，把本地的master分支和远程的master分支关联起来
git push -u origin master
```
### 创建并切换至分支
```
git checkout -b "dev"

// 相当于
git brnach dev
git checkout dev
```
### 查看分支
```
git branch
```
### 删除分支
```
git branch -d dev

// 强行删除
git branch -D dev
```
### 合并分支
```
// Fast-forward “快进模式”
git merge dev

// --no-ff, 禁用Fast forward
git merge --no-ff -m "merge with no-ff" dev
// 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
```
### 查看远程库信息
```
git remote -v
```
### 在本地创建和远程分支对应的分支
```
git checkout -b branch-name origin/branch-name
```
### 建立本地分支和远程分支的关联
```
git branch --set-upstream branch-name origin/branch-name
```
### 解决冲突
现在，master分支和feature1分支各自都分别有新的提交，变成了这样：
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/7.png)
这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，
解决冲突后，master分支和feature1分支变成了下图所示
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/8.png)
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
所以，团队合作的分支看起来就像这样
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/git/9.png)
### rebase操作
rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比
特点: 把分叉的提交历史“整理”成一条直线，看上去更直观。
缺点: 是本地的分叉提交已经被修改过了。
```
git rebase

git log --graph --pretty=oneline --abbrev-commit
```
原本分叉的提交现在变成一条直线了！这种神奇的操作是怎么实现的？其实原理非常简单。我们注意观察，发现Git把我们本地的提交“挪动”了位置，放到了f005ed4 (origin/master) set exit=1之后，这样，整个提交历史就成了一条直线。rebase操作前后，最终的提交内容是一致的，但是，我们本地的commit修改内容已经变化了，它们的修改不再基于d1be385 init hello，而是基于f005ed4 (origin/master) set exit=1，但最后的提交7e61ed4内容是一致的。
### 创建标签
```
git tag v1.0

git tag v0.9 f52c633

git tag

git show v0.9

// 创建带有说明的标签，用-a指定标签名，-m指定说明文字
git tag -a v0.1 -m "version 0.1 released" 1094adb
```
### 删除标签
```
git tag -d v0.1
```
### 推送标签到远程仓库
```
git push origin v1.0

git push origin --tags 

// 删除
git push origin :refs/tags/v0.9
```