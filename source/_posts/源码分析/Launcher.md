---
title: Android 源码Launcher模块分析
tags:
  - Launcher
  - 源码分析
categories:
  - Android
description: Launcher
toc: true 文章目录
abbrlink: a37bdd71
date: 2017-12-13 14:00:16
author:
comments:
original:
permalink:
---
## 提纲
1.什么是Launcher ,Launcher2 和 Launcher3的区别

2.Android 开机启动到Launcher运行的主要流程

3.Launcher 主要类介绍

4.Launcher 主要布局xml介绍

5.Launcher 一般主要需修改的地方

6.Launcher 动画，widget等

7.Launcher 主菜单界面UI

## Launcher启动的主要流程
手机开机->init进程->zygote进程->SystemServer->ActivityManagerService
->Launcher

init进程:

1.创建一些文件夹并挂载设备
2.初始化和启动属性服务 
3.解析init.rc配置文件并启动zygote进程

初始化init进程
system/core/init/init.cpp
启动zygote进程
system/core/rootdir/root.rc
{import /init.${ro.zygote}.rc}
不同的平台（32、64及64_32）
system/core/rootdir/init.zygote64.rc

zygote进程:
zygote 进程在初始化时会启动虚拟机，并加载一些系统资源。
这样 zygote fork 出子进程后，子进程也继承了能正常工作的虚拟机和各种系统资源，
接下来只需装载 apk 文件的字节码就可以运行应用程序了，可以大大缩短应用的启动时间，
这就是 zygote 进程的主要作用。


SystemServer： 
1.启动Binder线程池，这样就可以与其他进程进行通信。 
2.创建SystemServiceManager用于对系统的服务进行创建、启动和生命周期管理。 
3.启动各种系统服务。

Launcher
Launcher启动

1.Launcher.java:launcher中主要的activity。系统第一个启动的应用程序,在AndroidManifest.xml中定义了<category android:name="android.intent.category.HOME" />

2.LauncherApplication.java:应用程序全局初始化类,创建全局使用的应用程序缓存器Iconcache，创建全局使用的数据库加载类LauncherModel，注册事件监听器，注册数据库变化监听器

(在AndroidManifest.xml定义的Application)

3.IconCache.java:应用程序缓存器,通过一个HashMap<ComponentName,CacheEntry> mcache来缓存应用程序信息。内部类CachEntry对应存储应用程序的基本信息。makeDefaultIcon来创建默认的应用程序图标

4.LauncherModel.java:模型文件,封装了对数据库的操作。包含几个线程，加载所有应用程序和workspace的时候使用(loadAndBindAllApps，loadAndBindWorkspace)。其他的函数就是对数据库的封装，接口Callbacks提供加载程序和快捷方式的抽象方法。

5.LauncherProvider.java:launcher的数据库，里面存储了桌面的item的信息。在创建数据库的时候会调用loadFavorites(db)方法，解析xml目录下的default_workspace.xml文件，把其中的内容读出来写到数据库中，这样就得到了默认桌面的配置。

6.ItemInfo.java:对item的抽象，所有类型item的父类，item包含的属性有id（标识item的id）,cellX(在横向位置上的位置，从0开始),cellY（在纵向位置上的位置，从0开始） ,spanX（在横向位置上所占的单位格）,spanY（在纵向位置上所占的单位格）,screen（在workspace的第几屏，从0开始）,itemType（4种item类型，有widget，user_folder，application，shortcut）,container（三种item存放的地方desktop,application,user_folder）。

7.Workspace.java:抽象的桌面。由N个cellLayout组成, 从cellLayout更高一级的层面上对事件的处理。Launcher.java中通过bindItems添加cellLayout.实现了DropTarget, DragSource, DragScroller。既是拖拽源，又是拖拽目的地，还可以左右拖动。

8.CellLayout.java：桌面的某一屏。是组成workspace的view,被划分成4X4的cell空间，用boolean[][]mOccupied来标识每个cell是否被占用.

9.AllApp2D.java:显示和存储应用程序列表的视图。。android自带的AllApp2D包括一个GridView和一个HomeButton，MTK修改成PagerControl，HorizontalPager和4个imageview。

10.HorizontalPager.java:AllApp2D中间的网格控件。由N个GridView组成,AllApps2D.java中通过addGridView添加GridView。

11.DeleteZone.java:删除框。在平时是出于隐藏状态，在将item长按拖动的时候会显示出来，如果将item拖动到删除框位置时会删除item。DeleteZone实现了DropTarget和DragListener两个接口。

12. DragLayer.java:launcher.xml的rootview。DragLayer实际上也是一个抽象的界面，用来处理拖动和对事件进行初步处理然后按情况分发下去，角色是一个DragController。它首先用onInterceptTouchEvent(MotionEvent)来拦截所有的touch事件，如果是长按item拖动的话不把事件传下去，直接交由onTouchEvent()处理，这样就可以实现item的移动了，如果不是拖动item的话就把事件传到目标view，交有目标view的事件处理函数做相应处理。

13. DragController.java:为拖拽定义的一个接口。包含一个接口,一个方法和两个静态常量。

接口为DragListener（包含onDragStart()，onDragEnd()两个函数）,

onDragStart()是在刚开始拖动的时候被调用，

onDragEnd()是在拖动完成时被调用。

在launcher中典型的应用是DeleteZone，在长按拖动item时调用onDragStart()显示，在拖动结束的时候调用onDragEnd()隐藏。

一个函数包括用于在拖动是传递要拖动的item的信息以及拖动的方式

两个常量为DRAG_ACTION_MOVE，DRAG_ACTION_COPY来标识拖动的方式，DRAG_ACTION_MOVE为移动，表示在拖动的时候需要删除原来的item，

DRAG_ACTION_COPY为复制型的拖动，表示保留被拖动的item。

14.UserFolder.java: 用户创建的文件夹。实现了DropTarget，是拖拽目的地，可以将item拖进文件夹，单击时打开文件夹，暂不能处可以重命名文件夹。

15.LiveFolder.java:系统自带的文件夹。从系统中创建出的所有联系人的文件夹等。 

16.AddAdapter.java:长按桌面弹出的”添加到主屏幕”对话框所对应的适配器。。 

17.ShortcutAdapter.java:添加快捷方式的对话框所对应的适配器 

18.LauncherSettings.java:数据库项的字符串定义，


