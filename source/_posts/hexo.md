---
title: hexo常用命令
date: 2016-05-29 13:49:34
tags: hexo
categories: 2016
description:
toc: true 文章目录
author:
comments:
original:
permalink: /404
---

<!--more-->

## hexo常用命令

[使用hexo+github搭建属于自己的博客](http://www.jianshu.com/p/465830080ea9)

#### 1、新建
```java
hexo new "my blog" 
hexo n "my blog" #缩写
```


新建一篇文章，文章标题如果存在空格，需要使用双引号,新建的文件在 hexo/source/_posts/my-blog.md
#### 2、编译
```java
hexo generate 
hexo g #缩写
```
一般部署上去的时候都需要编译一下，编译后，会出现一个 public 文件夹，将所有的md文件编译成html文件 
#### 3、开启本地服务
```java
hexo server
hexo s #缩写
```
开启本地hexo服务,访问 localhost:4000
#### 4、部署
```java
hexo deploy
hexo d #缩写
```
部署到git上的时候，需要用这个命令
#### 5、清除
```java
hexo clean
hexo c #缩写
```
当 source 文件夹中的部分资源更改过之后，特别是对文件进行了删除或者路径的改变之后，需要执行这个命令，然后重新编译。 

#### 6、常用总结
```java
hexo n #写文章
hexo g #生成
hexo s #开启本地服务
hexo d #部署 #可与hexo g合并为 hexo d -g
```

以上都是一些常用的命令，命令详细请访问[官网](https://hexo.io/docs/commands.html)
