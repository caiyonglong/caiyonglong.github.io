---
title: 常用第三方库源码分析——EventBus
categories: 源码分析
tags:
  - Eventbus
abbrlink: 6ba9cd28
date: 2019-09-22 00:00:00
---
# 前言
EventBus是适用于Android和Java的发布/订阅事件总线。
![](/images/EventBus-Publish-Subscribe.png)

1、定义事件
2、准备订阅者，声明并注释订阅方法，也可以指定线程模式
注册和注销订阅者
3、发送事件

# 源码分析

注册方法，Eventbus实例注册订阅者事件
```
public void register(Object subscriber) {
        //获取注册的对象的类型
        Class<?> subscriberClass = subscriber.getClass();
        //查找并获取注册的对象的订阅方法
        List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriberClass);
        //加锁，在同步锁中订阅方法
        synchronized (this) {
            for (SubscriberMethod subscriberMethod : subscriberMethods) {
                //对订阅方法进行注册
                subscribe(subscriber, subscriberMethod);
            }
        }
    }
```

查找订阅者的订阅方法，METHOD_CACHE方法缓存，使用ConcurrentHashMap存储
```
List<SubscriberMethod> findSubscriberMethods(Class<?> subscriberClass) {
    //从缓存中获取订阅方法
    List<SubscriberMethod> subscriberMethods = METHOD_CACHE.get(subscriberClass);
    if (subscriberMethods != null) {
        return subscriberMethods;
    }

    //如果缓存中没有，则是否强制使用反射还是使用索引获取订阅方法
    if (ignoreGeneratedIndex) {
        subscriberMethods = findUsingReflection(subscriberClass);
    } else {
        subscriberMethods = findUsingInfo(subscriberClass);
    }
    if (subscriberMethods.isEmpty()) {
        throw new EventBusException("Subscriber " + subscriberClass
                + " and its super classes have no public methods with the @Subscribe annotation");
    } else {
        METHOD_CACHE.put(subscriberClass, subscriberMethods);
        return subscriberMethods;
    }
}
```