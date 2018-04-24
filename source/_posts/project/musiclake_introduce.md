---
title: 音乐湖应用介绍
tags: [MusicLake]
categories: Android
date: 2018-04-23 18:21:16
description: 项目具体功能介绍
toc: true 文章目录
author:
comments:
original:
permalink:
---
# MusicLake
Android 音乐播放器，基于MVP + Retrofit + Rxjava2 框架。代码重构了一次，还是发现还有很多不足的地方。

# 已完成功能

- [x] 本地听歌。按专辑、歌手、文件夹分类
- [x] 在线听歌。百度音乐，QQ音乐、虾米音乐、网易云音乐
- [x] 本地歌单
- [x] 播放历史
- [x] 播放队列
- [x] 网络歌曲下载
- [x] 播放歌词、桌面歌词
- [x] 通知栏控制，线控播放
- [x] 音频焦点控制
- [x] QQ登录、在线歌单同步

# 计划
- [ ] 优化项目架构
- [ ] 验证，修复bug
- [ ] 考虑将音乐播放封装成一个库
- [ ] 封装api

# release note
[点这下载](https://github.com/caiyonglong/MusicLake/releases)

# 音乐来源
- 绝大部分来源于[Binaryify/NeteaseCloudMusicApi](https://github.com/Binaryify/NeteaseCloudMusicApi)和[LIU9293/musicAPI](https://github.com/LIU9293/musicAPI)等
- [sunzongzheng/musicApi](https://github.com/sunzongzheng/musicApi)

# 项目经历
- 因为接口的问题，通过github，发现一些音乐资源开源库（py,js），通过分析这些调用，用java实现了一个类似的接口。
- 项目重构经验，解决重构项目时遇到的问题也是一种进步。
- 项目一开始是一个基于LBS的音乐社区软件，由于服务器方面问题，后台接口全部用不了，所以为了能够使软件正常运行，
将涉及后台社区以及地图方面的功能全不裁剪了，只剩下音乐播放的功能，功能虽然看起来比较少。但是作为一个练手项目，也足够了。
