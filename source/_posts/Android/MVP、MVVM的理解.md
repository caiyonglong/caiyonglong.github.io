
---
title: MVC ，MVP，MVVM 的理解
date: 2019-08-31
categories: Android
---

一切都是为了解耦合，开发更加方便

# MVC简单介绍

Model、View、Controller 
Model 处理网络数据，数据库数据等
View xml
Controller activity

# MVP

Model View Presenter

Model 处理网络数据，数据库数据等

Presenter 主要把model 和 view 隔离，减少耦合，拆分Controller层

# MVVM
基于MVP模式的基础上，将P变成ViewModel，使用databinding 实现数据的双向绑定。

Viewmodel
## Databinding 
[Google 官方介绍](https://developer.android.com/topic/libraries/data-binding)
官方解释：
数据绑定库是一个支持库，允许您使用声明性格式而不是以编程方式将布局中的UI组件绑定到应用程序中的数据源。

布局通常在具有调用UI框架方法的代码的活动中定义。 例如，下面的代码调用findViewById（）来查找TextView小部件并将其绑定到viewModel变量的userName属性：

DataBinding 是谷歌官方发布的一个框架，顾名思义即为数据绑定，是 MVVM 模式在 Android 上的一种实现，用于降低布局和逻辑的耦合性，使代码逻辑更加清晰。MVVM 相对于 MVP，其实就是将 Presenter 层替换成了 ViewModel 层。DataBinding 能够省去我们一直以来的 findViewById() 步骤，大量减少 Activity 内的代码，数据能够单向或双向绑定到 layout 文件中，有助于防止内存泄漏，而且能自动进行空检测以避免空指针异常

启用 DataBinding 的方法是在对应 Model 的 build.gradle 文件里加入以下代码，同步后就能引入对 DataBinding 的支持

```
android {
    dataBinding {
        enabled = true
    }
}
```

## LiveData

LiveData是一个数据持有类，它可以通过添加观察者被其他组件观察其变更。不同于普通的观察者，它最重要的特性就是遵从应用程序的生命周期，如在Activity中如果数据更新了但Activity已经是destroy状态，LivaeData就不会通知Activity(observer)。当然。LiveData的优点还有很多，如不会造成内存泄漏等。
LiveData通常会配合ViewModel来使用，ViewModel负责触发数据的更新，更新会通知到LiveData，然后LiveData再通知活跃状态的观察者。
