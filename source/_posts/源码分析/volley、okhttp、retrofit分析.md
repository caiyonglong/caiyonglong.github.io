---
title: 常用网络请求库对比——Volley、Okhttp、Retrofit对比
categories: 源码分析
abbrlink: b0301fc2
date: 2019-09-22 00:00:00
tags:
---

Volley、Okhttp、Retrofit三大网络请求库分析

# 1. 原生

Android 原生使用HttpUrlConnection

# 2. Volley

Volley 是 Google 官方出的一套小而巧的异步请求库，该框架封装的扩展性很强，支持 HttpClient、HttpUrlConnection，甚至支持 OkHttp，具体方法可以看 Jake 大神的这个 Gist 文件

## 优点


## 缺点
在BasicNetwork中判断了statusCode(statusCode < 200 || statusCode > 299)，如何符合条件直接抛出IOException()，不够合理

导致401等其他状态抛出IOException 
解决方案 : 
http://blog.csdn.net/kufeiyun/article/details/44646145 
http://stackoverflow.com/questions/30476584/android-volley-strange-error-with-http-code-401-java-io-ioexception-no-authe

图片加载性能一般

对大资源下载支持不够
