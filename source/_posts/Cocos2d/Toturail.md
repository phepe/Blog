---
title: Coco2dx编译记录
date: 2018-08-01 14:25:21
categories: 
    - Game
tags:
    - Cocos2dx
---

### 1. 升级SDK

当我们升级`cocos2d/cocos/platform/android/libcocos2dx`的SDK版本的时候有可能会发生以下错误

    .../Cocos2dxDownloader.java:9: error: package org.apache.http does not exist
    
这是因为在当Aandroid的SDK高于22的时候Apache的`HttpClient`已经被谷歌废弃掉了。

<!-- more -->
解决办法有2种:

* 在`/cocos2d/cocos/platform/android/libcocos2dx/build.gradle`文件里面加入以下代码指定使用废弃的`http`安装包

        android {
            useLibrary 'org.apache.http.legacy'
        }
* 手动将对应版本的SDK的`http`包文件`android-sdk/platforms/android-28/optional/org.apache.http.legacy.jar`复制到项目的`/cocos2d/cocos/platform/android/java/libs`目录下。
 
### 2. 升级gradle

我们在使用新的SDK的时候有时需要升级对应的`gradle`，例如我们使用新版本的Admob的时候

    compile 'com.google.firebase:firebase-ads:15.0.0'
必须使用新的`gradle`才能找到对应的包，在`proj.android-studio/gradle/wrapper/gradle-wrapper.properties`的`distributionUrl`字段设置使用的`gradle`版本。
然后在`proj.android-studio/build.gradle`里面设置Gradle插件的的版本。

在虚拟机里面升级`gradle`后有时候会导致编译失败。在`xp`系统里面无法使用新的`gradle`