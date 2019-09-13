---
title: Dagger2 分析
date: 2019-08-15 10:04:02
categories: Android
tags: 
---


# Dagger2 是什么？
Dagger2是Dagger的升级版，是一个依赖注入框架,第一代由大名鼎鼎的Square公司共享出来，第二代则是由谷歌接手后推出的，现在由Google接手维护.

Google dagger [Github 地址](https://github.com/google/dagger)

Dagger是依赖注入的编译时框架。 它不使用反射或运行时字节码生成，在编译时进行所有分析，并生成纯Java源代码。

Gladle引入
```
// Add Dagger dependencies
dependencies {
  api 'com.google.dagger:dagger:2.x'
  annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
}
```