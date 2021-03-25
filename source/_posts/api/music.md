---
title: 音乐接口
tags:
  - api
categories: 接口
description: 音乐接口总结
toc: true 文章目录
abbrlink: 28ffda2f
date: 2016-08-25 08:21:16
author:
comments:
original:
permalink:
---
有点想总结一下音乐接口，

## 歌曲来源：百度音乐


详情请访问原文[http://blog.csdn.net/xyw_blog/article/details/38641793?utm_source=tuicool&utm_medium=referral](http://blog.csdn.net/xyw_blog/article/details/38641793?utm_source=tuicool&utm_medium=referral)

获取百度音乐排行榜的数据
http://musicapi.qianqian.com/v1/restserver/ting?from=android&version=6.0.7.1&channel=huwei&operator=1&method=baidu.ting.billboard.billCategory&format=json&kflag=2


获取频道列表数据
http://fm.baidu.com/dev/api/?tn=channellist
根据频道id获取频道音乐数据列表信息
http://fm.baidu.com/dev/api/?tn=playlist&id=public_tuijian_rege
根据songIds获取歌曲信息，如果要获取多首信息，包括歌曲下载链接地址，songIds之间用逗号隔开，如songIds = 913288,872633,3388338
http://fm.baidu.com/data/music/songlink?songIds=913288
根据songIds获取歌曲信息，如果要获取多首信息，songIds之间用逗号隔开，如songIds = 913288,872633,3388338 http://fm.baidu.com/data/music/songinfo?songIds=913288


## 歌曲来源：QQ音乐
来源于github 开源库[MusicAPI](https://github.com/LIU9293/musicAPI)
## 歌曲来源：虾米音乐
来源于github 开源库[MusicAPI](https://github.com/LIU9293/musicAPI)
## 歌曲来源：网易云音乐
来源于github 开源库[NeteaseCloudMusicApi](https://binaryify.github.io/NeteaseCloudMusicApi/#/)

# 备注
因为这些音乐接口都是加密的，而这些开源库是使用py或者js的一些解密算法解密，所有并不能直接调用这些音乐接口，还得解密。
# 计划
kotlin解析音乐API，生成一个库文件
