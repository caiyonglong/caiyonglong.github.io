---
title: git使用---修改历史提交记录
date: 2018-2-02 21:34:25
description: 
categories: 2018
tags: [git]
author:
comments:
original:
permalink: 
---

# git cherry-pick所遇到的问题

使用git 也有很长一段时间了，所以想总结一下git 的使用。其中，在日常工作中copy代码（搬砖）是常有的事情。对于git 管理的分支来说，git cherry-pick就是另类的copy。说的正式点就是 **移植**。

而**cherry-pick** 就是移植命令。

--对于git cherry-pick过来的提交记录，有些是因为一点小错误导致重复提交的，还有一些是一个功能分了好几次提交，对于这些记录，如果全部按原来分支的提交记录提交，就感觉很麻烦，到底有没有一种方法能够合并几条提交记录呢？

还有就是当cherry-pick多条记录后，突然想修改其中一条修改记录时。发现很难办。这时 git rebase 就很轻松了解决掉了这些难处。

当然网上也有很多类似的介绍，也可以去借阅一下。

## 1. 合并多条修改


初始时，通过git log 查看 提交记录，记录一下commit hash值。

```
@ubuntu:~/work/git$ git log
commit e916213e3c4cc9c977ec20902ae27ae45352eef3
Date:   Tue Feb 6 20:05:32 2018 +0800

    Third commit

commit 358371a2b2b15d847bf89e44e94e7b926fed2da0
Date:   Tue Feb 6 20:04:48 2018 +0800

    Second commit

commit ba82e7007559afab14ff124ea51e19772dedfe2c
Date:   Tue Feb 6 20:03:32 2018 +0800

    First commit

```
然后通过 git rebase -i commit-id 命令编辑提交记录。

- git rebase  -i commit-id 表示，修改commit-id以后的提交记录，
- 也可以用git rebase -i HEAD ~n. n表示倒数第几条记录。

```
pick 358371a Second commit
pick e916213 Third commit

# Rebase ba82e70..e916213 onto ba82e70
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#

```
其中

- pick 使用这条提交（保持不变）
- reword 使用这条提交，可以编辑当前commit信息
- edit 使用这条提交，放弃当前commit信息
- squash 使用这条提交，合并前一条提交
- fixup 和squash一样，但会放弃当前这个提交的信息
- exec 

合并修改使用，将 e916213 Third commit 前的pick 改成squash 即可，然后保存退出。

```
pick 358371a Second commit
squash e916213 Third commit
```
修改完成后再次查看git log，已经合并到一起了。因为使用squash，所以两条记录得message都会合在一起。如果 使用fixup，则会放弃Third commit 的commit 信息。
```
commit 940fec9ea88c69a195c546d48e5209c4cdb6abec
Date:   Tue Feb 6 20:04:48 2018 +0800

    Second commit
    
    Third commit

commit ba82e7007559afab14ff124ea51e19772dedfe2c
Date:   Tue Feb 6 20:03:32 2018 +0800

    First commit

```
## 2. 修改提交记录中的commit 信息
同上面，只要在需要修改的记录前 将pick 换成reword，即可进入编辑界面，编辑commit message，然后保存退出。这样就修改完成了。

当使用edit时，虽然也可以修改，
```
Stopped at 3af42b9... reword Second commit
You can amend the commit now, with

	git commit --amend

Once you are satisfied with your changes, run

	git rebase --continue

```
- 再使用**git commit --amend** 进入编辑界面修改。
- 保存，使用**git rebase --continue** 返回到最新节点。

可以实际操作一下，如果怕玩坏工作分支，可以自己新建一个分支玩一玩。
这个git rebase 真的很神奇。对于rebase的其他功能也很强大。