---
title: Android面试
hide: true
abbrlink: 7e8ea226
date: 2018-04-27 23:37:10
tags:
---

## Android面试
1、Android 常用的设计模式有哪些，适配器模式
单例模式、适配器模式、工厂方法模式、Builder模式、
2、Android Studio 常用的一些优化方案
优化编译速度，修改配置文件，gradle本地本地构建

3、手写mvp模式的一种实现方案
我目前写的mvp主要是 契约类定义View和Presenter的一些方法接口，
然后present类实现契约类里面的preseter接口，实现具体的方法，处理数据逻辑，并调用view的接口方法
activity 实现契约类 view的方法。
 
4、Android内存优化方案

单例导致内存泄露
静态变量导致内存泄露
非静态内部类导致内存泄露
未取消注册或回调导致内存泄露
Timer和TimerTask导致内存泄露
集合中的对象未清理造成内存泄露
资源未关闭或释放导致内存泄露
属性动画造成内存泄露
WebView造成内存泄露

构造单例的时候尽量别用Activity的引用；
静态引用时注意应用对象的置空或者少用静态引用；
使用静态内部类+软引用代替非静态内部类；
及时取消广播或者观察者注册；
耗时任务、属性动画在Activity销毁时记得cancel；
文件流、Cursor等资源及时关闭；
Activity销毁时WebView的移除和销毁。

5、自定义View的绘制流程

1. 自定义View的属性；

2. 在View的构造方法中获得自定义的属性；

3. 重写onMeasure()； --> 并不是必须的，大部分的时候还需要覆写

4. 重写onDraw()；

6、View的事件分发机制

Activity-> ViewGroup ->View
dispatchTouchEvent() 负责分发 =>true被消费掉 -> false继续往下传递 


7、使用了哪些三方框架 

8、谈一下Android的动画
https://www.jianshu.com/p/2412d00a0ce4
9、rxbus2 是怎么实现的
基于rxjava的flowable ,观察着模式
10、Activity的启动流程
桌面上点击会调用Launcher的startActivity，=>最后调用系统的startactivityforResult 然后在通过Binder与AMS进行进程间通信，ams fork一个新的进程出来
11、侧滑菜单的实现原理

12、service的两种启动方式
13、fragment与activity的通信方式
14、ArrayList 和 LinkList的区别
ArrayList 内部使用数组的形式实现了存储，实现了RandomAccess接口，利用数组的下面进行元素的访问，因此对元素的随机访问速度非常快。
LinkedList：内部使用双向链表的结构实现存储，LinkedList有一个内部类作为存放元素的单元，里面有三个属性，用来存放元素本身以及前后2个单元的引用，另外LinkedList内部还有一个header属性，用来标识起始位置，LinkedList的第一个单元和最后一个单元都会指向header，因此形成了一个双向的链表结构。

15、abstract 和 interface 

相同点：
1.两者都是抽象类，都不能实例化
2.interface 实现类及abstract class 的子类都必须要实现已经声明的抽象方法。
不同点：
1.interface 实现，要用implements，而abstract class的实现，要用extends.
2.一个类可以实现多个interface,但一个类只能继承一个abstract class.
3.interface强调特定功能的实现，而abstract class强调所属关系
4.尽管interface实现类及abstract class的子类都必须要实现相应的抽象方法，但实现的形式不同。interface中的每一个方法都是抽象方法，都只是声明的（declaration,没有方法体），必须要实现。而abstract class的子类可以有选择地实现。

抽象类的这个选择有两点含义：一是Abstract class中并非所有的方法都是抽象的，只有那些冠有abstract的方法才是抽象的，子类必须实现。那些没有abstract class的方法，在abstract class中必须定义方法体。二是abstract class的子类在继承它时，对非抽象方法即可以直接继承，也可以覆盖；而对抽象方法，可以选择实现，也可以通过再次声明其方法为抽象的方式，无需实现，留给其子类来实现，但此类必须也声明为抽象类，即是抽象类，当然也不能实例化。
interface是完全抽象的，只能声明方法，而且只能声明public的方法，不能声明private 及protected的方法，不能定义方法体，也不能声明实例变量。

