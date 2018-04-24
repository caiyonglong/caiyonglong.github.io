---
title: 客户化定制开机logo、开机动画
date: 2017-11-02 21:34:25
description: 
categories: Android
tags: [定制]
author:
comments:
original:
permalink: 
---

## 设置开机logo
### 高通平台
```
1、修改的文件路径
LINUX/android/bootable/bootloader/lk/splash
2、准备好logo图片(png、bmp格式)
3、查看中原图片的分辨率，修改logo图片*保证分辨率一致*
4、生成splash.img镜像文件
```
生成splash 步骤
```
The steps to generate a splash.img:
1 sudo apt-get install python-imaging
2 python ./logo_gen.py boot_001.png (*.bmp)
```
### MTK平台
```
1、修改的文件路径
vendor\mediatek\proprietary\bootable\bootloader\lk\dev\logo
2、准备好logo图片(bmp格式)
3、查看中原图片的分辨率，修改logo图片*保证分辨率一致*替换相应格式的logo

```
## 设置开关机动画
首先自己制作开关机动画包，如果有现成的压缩包那就不用了
，压缩包的压缩方式必须是存储方式的zip格式压缩。
```
bootanimation.zip
shutdownanimation.zip
```
压缩包中有两个文件夹part0、part1和desc.txt文件

part0和part1中主要存放一些开机动画的帧图片

desc.txt中内容如下

```
720 1680 15
p 1 0 part1
p 0 0 part0

```
 - 720 1680 代表开机动画图片分辨率
 - 15 代表播放速度。15 帧每秒
 - p 1（代表着播放一次） 0（空指令）part1 代表这part1文件夹内的图片只按名称顺序播放一次
 - p 0（代表着播放一次） 0（空指令）part0  代表这part0文件夹内的图片只按名称顺序循环播放直到结束

预览：必须在已经root的手机下才能替换。
```
//进入root权限
adb root
adb remount 
adb shell 
//进入开关机动画存放目录
cd system/media 
//删除手机原先的开关机动画
rm bootanimation.zip
rm shutdownanimation.zip 
//将开关机动画push到手机
adb push 文件路径 system/media
```
最后重启手机即可看到效果
Android 源代码中替换开关机压缩包
```
高通平台
LINUX/android/vendor/qcom/proprietary/qrdplus/Extension/apps/BootAnimation
MTK平台
framework/base/data/sounds
```

