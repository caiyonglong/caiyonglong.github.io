---
title: 音乐湖环境部署
tags:
  - MusicLake
categories: Android
toc: true 文章目录
abbrlink: musicLakeBuild
date: 2021-03-29 18:21:16
author:
comments:
original:
permalink:
---
# 音乐湖环境部署

音乐湖项目主要包含了Android端，PC端，微信小程序端，歌单服务器等几个项目，具体项目地址如下：

[Android客户端 MusicLake](https://github.com/caiyonglong/MusicLake)
[PC端 music](https://github.com/sunzongzheng/music)
[云歌单服务器 play-be](https://github.com/sunzongzheng/player-be)
[音乐解析 Api](https://github.com/sunzongzheng/musicApi)

## 前言
因为涉及音乐版权问题，被官方警告了。所以我们就不再提供可用的应用以及接口API了，目前主要偏向于学习研究。在群里也有人反馈看不懂README，部署比较困难，看到别人的部署博客，非常详细，非常好，所以作为开发者的我们也应该写一篇来记录记录。
如果只是想用单纯的使用音乐湖软件听歌，不建议搭建这一套环境体系。

## [项目介绍](./musicLake.html)

本项目采用CS架构，客户端-服务器模式，搭建部署整个音乐播放服务。

客户端：MusicLake、PC客户端
服务器：play-be（登录、云歌单）、NeteaseMusicApi（网易云音乐api）

## 云歌单服务器部署

[云歌单服务器 play-be](https://github.com/sunzongzheng/player-be)

### 开发
cp config/default.js config/local.js
修改config/local.js相应配置
npm install
npm run start

### 部署
- cp config/default.js config/local.js
- 修改config/local.js相应配置
- 以下三种方式任选一
```
docker-compose up -d # Docker Compose（Recommended）
pm2 start pm2.production.json # PM2, daemon run
npm run start # Just run it
```

## PC客户端部署
如果想直接使用体验，github 支持`workflow`部署，release下有打包好的生成包，可以直接选择对应的客户端下载安装即可。

也可以自己打包

### 打包环境要求
- nodejs v12.1.0 版本及以上
- yarn

### 打包
```
yarn run build
```
更多请见 [github](https://github.com/sunzongzheng/music/blob/master/build.md)

## Android客户端部署

未完待续...

## Github登录服务配置

未完待续...

## QQ登录配置

这个需要备案的域名，才能使用。建议推荐Github登录配置



