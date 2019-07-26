---
title: Coco2dx游戏Android升级64位
date: 2019-07-21 22:54:21
categories:
    - Game
tags:
    - Cocos2dx
---

### 1. 概述

谷歌要求所有使用的Native的游戏在2019-08-01之前需要升级支持64位，而我以前的游戏没有编译64位的so文件。
<!-- more -->
### 2. 添加`arm64-v8a.so`的库

由于我游戏使用的是`cocos2d-x-3.11.1`，不支持Android 64位，需要从最新的`cocos2d-x-3.17.2`里面将一些64位的so复制过来（`external\chipmunk\prebuilt\android`），主要涉及到以下so：
* freetype2
* png
* zlib
* jpeg
* tiff
* webp
* chipmunk
* websockets

用python写了一个工具来复制这些库


### 3.修改Android项目的配置

* 在Application.mk中添加（armeabi可以不要）`gradle.properties`以及`build.gradle`里面都可以不修改的：

      APP_ABI := armeabi-v7a arm64-v8a

* 修改`\cocos2d\cocos\platform\android\CCEnhanceAPI-android.cpp`文件，注释`size_t __ctype_get_mb_cur_max`的定义，不然会和 `CCApplication-android.cpp` 有重复定义的问题,

* 将NDK升级到`android-ndk-r12b`,不然编译的时候老的NDK会缺少对应的64位的库，如果用NDK10 就需要替换`libc.so`由于使用的是低版本的NDK所以需要替换NDK目录下的`platforms\android-21\arch-arm64\usr\lib\libc.so`，通过`Android Sutido`创建`arm`平台的虚拟机`/system/lib64/libc.so`取出，替换就行。但是能用新的就用新的吧，虽然编译出来后的包会大一点

* 谷歌的广告不嫩用`18.1`只能用`17.1`,不然玩了其他加了广告的游戏，再切换回我的游戏就会等一会就会蹦，谷歌后台有很多蹦的日志。
