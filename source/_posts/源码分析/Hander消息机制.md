---
title: Handler消息机制
date: 2019-09-22
categories: 源码分析
tags: [Handler]
---

# 前言
平时工作中缺乏总结，写博客不是为别人，而是为自己，虽然网上很多类似的，但是自己还是得写一遍加深印象

# 基本原理

消息机制包含 Handler、MessageQueue、Message、Looper

拖欠图

Handler 发送消息，消息存入到消息队列中，然后Looper 循环执行消息队列。




# 问题

## Handler 为什么会造成内存泄漏
因为在Activity中，handler内部类引用了它，而该handler实例可能被MessageQueue引用着。比如发送了一个延时消息到队列中，那么就可能在队列中存在很长时间，而消息队列（MessageQueue）的生命周期等于它所在的线程。当大到Activity被finish()了后还在队列中时，就满足了上面的短生命周期引用长生命周期的条件。根据Java GC的规则，Activity的引用计数不为0，故不会回收
- 解决办法
    
    1、保证Activity被finish()时该线程的消息队列没有这个Activity的handler内部类的引用
    2、要么让这个handler不持有Activity等外部组件实例，让该Handler成为静态内部类。（静态内部类是不持有外部类的实例的，因而也就调用不了外部的实例方法了）
    3、在2方法的基础上，为了能调用外部的实例方法，传递一个外部的弱引用进来

## 为什么消息Handler能实现跨线程通信

## 为什么Looper循环不会导致ANR

ANR的基本概念

    1：5s内无法响应用户输入事件(例如键盘输入, 触摸屏幕等)
    2：BroadcastReceiver在10s内无法结束
    3：ServiceTimeout(20s) --小概率类型，Service在特定的时间内无法处理完成

Android是事件驱动的，在Loop.loop()中不断接收事件、处理事件，而Activity的生命周期都依靠于主线程的Loop.loop()来调度，所以可想而知它的存活周期和Activity也是一致的。当没有事件需要处理时，主线程就会阻塞；当子线程往消息队列发送消息，并且往管道文件写数据时，主线程就被唤醒。

主线程在没有事件需要处理的时候就是处于阻塞的状态。想让主线程活动起来一般有两种方式：

- 第一种是系统唤醒主线程，并且将点击事件传递给主线程；
- 第二种是其他线程使用Handler向MessageQueue中存放了一条消息，导致loop被唤醒继续执行。

主线程Looper从消息队列读取消息，当读完所有消息时，主线程阻塞。子线程往消息队列发送消息，并且往管道文件写数据，主线程即被唤醒，从管道文件读取数据，主线程被唤醒只是为了读取消息，当消息读取完毕，再次睡眠。因此loop的循环并不会对CPU性能有过多的消耗。

总结： Looer.loop()方法可能会引起主线程的阻塞，但只要它的消息循环没有被阻塞，能一直处理事件就不会产生ANR异常。

主线程Looper在哪里初始化

ActivityThread 类里面

```
 public static void main(String[] args) {
        Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "ActivityThreadMain");

        // CloseGuard defaults to true and can be quite spammy.  We
        // disable it here, but selectively enable it later (via
        // StrictMode) on debug builds, but using DropBox, not logs.
        CloseGuard.setEnabled(false);

        Environment.initForCurrentUser();

        // Set the reporter for event logging in libcore
        EventLogger.setReporter(new EventLoggingReporter());

        // Make sure TrustedCertificateStore looks in the right place for CA certificates
        final File configDir = Environment.getUserConfigDirectory(UserHandle.myUserId());
        TrustedCertificateStore.setDefaultUserDirectory(configDir);

        Process.setArgV0("<pre-initialized>");


        Looper.prepareMainLooper();

        // Find the value for {@link #PROC_START_SEQ_IDENT} if provided on the command line.
        // It will be in the format "seq=114"
        long startSeq = 0;
        if (args != null) {
            for (int i = args.length - 1; i >= 0; --i) {
                if (args[i] != null && args[i].startsWith(PROC_START_SEQ_IDENT)) {
                    startSeq = Long.parseLong(
                            args[i].substring(PROC_START_SEQ_IDENT.length()));
                }
            }
        }
        ActivityThread thread = new ActivityThread();
        thread.attach(false, startSeq);

        if (sMainThreadHandler == null) {
            sMainThreadHandler = thread.getHandler();
        }

        if (false) {
            Looper.myLooper().setMessageLogging(new
                    LogPrinter(Log.DEBUG, "ActivityThread"));
        }

        // End of event ActivityThreadMain.
        Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
        Looper.loop();

        throw new RuntimeException("Main thread loop unexpectedly exited");
    }
    ```

