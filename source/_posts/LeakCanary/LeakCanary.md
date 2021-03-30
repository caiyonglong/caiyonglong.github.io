---
title: LeakCanary 集成和原理分析
abbrlink: LeakCanary
categories: 内存优化
date: 2021-03-30 22:35:00
---
最近看到项目中用到的LeakCanary版本还是1.6.3版本，所以想升级一下。看到LeakCanary的官方文档，最新版已经是2.7了，所以想记录一下以及分析一下新版的原理

## 介绍
[LeakCanary](https://square.github.io/leakcanary/)是适用于Android的内存泄漏检测库。

官方解释：

>LeakCanary’s knowledge of the internals of the Android Framework gives it a unique ability to narrow down the cause of each leak, helping developers dramatically reduce OutOfMemoryError crashes.


LeakCanary可以帮忙定位内存泄漏问题，从而帮助开发人员大大减少OutOfMemoryError崩溃。

集成到项目中后，一般是测试包和开发包会用到，生产包不需要依赖。

## 集成

在**app/build.gradle** 添加一下依赖

```
dependencies {
  // debugImplementation because LeakCanary should only run in debug builds.
  debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.7'
}
```

可以在 **Logcat** 里面通过过滤搜索 **LeakCanary**，来判断LeakCanary是否运行

```
LeakCanary: LeakCanary is running and ready to detect leaks
```



### 1.6.版本和2.0版本差别对比

对比老版本`1.6.3`和新版本 `2.7`。主要的更新点如下（官方文档比较多，这里只写一下主要的点）：

- 集成更加方便，原来需要一次集成多个依赖库，新版只需要集成一个依赖
- 内存泄漏支持分组，相同的内存泄漏会自动分为一类，方便查看
- 使用Kotlin重写
- 新的堆解析器，比旧版减少了90%内存，速度快了6倍。个人体验确实比1.6.3版本快了很多



## 原理分析

未完待续...