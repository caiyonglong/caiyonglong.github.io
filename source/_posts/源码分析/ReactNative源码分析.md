---
title: ReactNative源码分析
tags:
  - ReactNative
date: '2021-05-06 18:15'
abbrlink: aa74f4e0
---

### 前言

最近一个月都在看RN的问题，因为线上RN包出现一个偶现的崩溃问题，在react-native github上也没人回复。搞得头都大了，每天都是查埋点，抓日志。想找出一个复现的具体步骤，但是根据埋点和日志信息都没能复现定位到线上崩溃问题。所以想通过源码集成的方式，能在RN Android源码层面加一些日志来辅助定位。随便看看RN源码是怎么实现JS和Android之间的交互和通信的

### ReactNative源码集成
1、下载React-Native源代码

```git
git clone https://github.com/facebook/react-native

git tag v0.64.0
```

切换到对应版本，react-native是通过git tag来区分版本的。所以代码拉取完成后需要通过git tag来切换到对应的版本上。本文是基于**0.64**版本

2、npm安装环境

在react-native根目录下，通过npm安装依赖

```npm
npm install
```

3、通过Android Studio打开**react-native/ReactAndroid**。编译即可

react-native官网上具体的集成方式：[从源代码编译React Native](https://reactnative.cn/docs/building-from-source)



#### React-Native源码分析

主要分析ReactNative的启动流程、Android端的渲染原理、通信机制等方面

- [ReactNative的启动流程]()
- 渲染原理
- 通信机制

