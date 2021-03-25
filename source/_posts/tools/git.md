---
title: git使用---修改历史提交记录
categories: 开发工具
tags:
  - git
abbrlink: be0c7f93
date: 2018-02-02 21:34:25
description:
author:
comments:
original:
permalink:
---

# git使用 总结

@(总结)[git]

总结一下当前工作中经常使用的git命令

##日常使用

### git初始化和拉取git分支
```
git init
git clone ...
```
### 修改&提交
对于只有修改前先pull 一下代码，修改完成后再push
```
git add -u / -A 
git commit 
git push 
```
### 分支管理

```
git branch 
git branch newbranch
git checkout newbranch
```

### 查看状态&历史记录
```
git status
git log 
```
### 移植
```
git cherry-pick 
git commit --amend
```


## 常用命令

### git log

- git log 查看 **当前分支** 的提交记录
- git log --stat
- git log -5 --pretty --oneline 显示过去5次提交
- git log --follow <path> 显示某个文件的版本历史，包括文件改名

### git status
显示git状态
主要分为工作区和暂存区两个，其中工作区又分为git 监控的和未被监控的两个区域。

### git add 

- git add filename 
- git add -A/.  添加当前目录 all tracked and untracked files 到暂存区
- git add -u 添加当前目录的tracked files 到暂存区

### git commit 
将暂存区的提交当分支上

- git commit -m "the commit message" 提交
- git commit --amend 增补提交. 会使用与当前提交节点相同的父节点进行一次新的提交,旧的提交将会被取消.

### git checkout
可以移出**暂存区**的文件、可以切换分支也可以切换到tag节点。

- git checkout branch 检出分支
- git checkout tag_name 检出当前tag
- git checkout file_name 放弃对文件的修改

### git branch
可以用来创建、删除、查看分支，不管是远程的还是本地的都可以。

- git branch 显示本地仓库分支
- git branch -a 查看本地远程仓库所有分支
- git branch -r 显示远程仓库分支
- git branch -D master 删除master分支
- git branch master 新建master分支

### git reset
用来回退版本，例如从当前commit 回退到历史提交中

- git reset --hard <commit> 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
- git reset --soft <commit> 不会改变暂存区，仅仅将commit回退到了指定的提交
- git reset --mixed <commit> 重置当前分支的HEAD为指定commit，工作区commit之后的全部放入暂存区

### git clean
可以用来清理工作区中 Untracked files。 

- git clean -n 显示 将要 删除的 文件 和  目录
- git clean -f 删除 文件
- git clean -df 删除 文件 和 目录


### git pull
从服务器拉取代码，也有些说法不推荐使用pull拉取代码，因为拉取的代码有可能会污染本地的提交记录。


### git push
将本地分支推送到服务器上

- git push origin master 将本地分支推送到master远程分支

### git cherry-pick
将其他分支的提交修改剪切到当前分支，cherry-pick 后面带上需要剪切移植的commit ID。当遇到cherry-pick冲突后，
需要解决冲突，然后使用 git commit --amend 修改提交信息即可。
- git cherry-pick <commit> 剪切过来提交过来后，直接提交到当前分支
- git cherry-pick --no-commit <commit> 不直接提交当前分支，在暂存区可见修改记录的文件。

### git merge

- git merge master 当前分支合并 master分支

### git diff

显示暂存区和工作区的差异

### git tag

- git tag <tag> 新建一个tag在当前commit
- git tag <tag> <commit> 新建一个tag在指定commit
- git tag -d <tag> 删除本地tag
- git push origin :refs/tags/<tag> 删除远程tag

### git rebase

### git stash

- git stash save -a "message" 将当前改动保存到一个
- git stash list 显示所有临时区的stash id
- git stash pop  
- git stash drop  




## gitk 图形界面

**gitk** 是一个图形界面，可用用来查看提交记录和修改记录。
也可以使用其他的git 管理工具，例如source tree。感觉挺好用的。



