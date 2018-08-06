---
title: WMware coco2dx环境安装
date: 2018-08-06 16:16:10
categories: 
    - Basic Skills
tags:
    - WMware
---

### 1. 安装虚拟机

如果想要兼容新版本的Android Studio，最好下载64位的Win7系统，VMware Fusion的版本使用的是10.0.1。然后按照以下步骤创建虚拟机：

* 新建Win7虚拟机，选择下载下来的ISO镜像，内存建议4G，硬盘40G。
* 将CD/DVD设置成IDE，不然会报错`nor find file win7.gho`。
* 使用PQ/PM设置系统分区，类型选择NTFS，然后进阶，设置为作用
* 从CD/DVD启动，选择安装到第一分区，进行安装。
* 安装完成后安装VMware Tools，点击安装后如果没有弹出安装界面，需要手动在我的电脑里面找到`setup.exe`文件，点击安装。

<!-- more -->

### 2. 安装Android开发环境

需要准备Android Studio和JDK安装包:

* 在谷歌的官方网站上下载Android Studio，进行安装，最好不要安装在有空格的目录下，因为Cocos2dx识别环境变量的时候不能有空格。

* 安装完成后如果提示没有Java环境，则需要去Orecal的Java官方网站上下载JDK进行安装，然后将JDK的路径设置到`Path`环境变量中。如果没有Path环境变量，则新建一个。

        Path    C:\Program Files\Java\jdk1.7.0_67\bin;

### 3. 安装Cocos2dx开发环境

#### 3.1 安装Python

在Python官方网站下载最新的Python2.7版本，注意要下载对应系统位数的版本，然后进行安装，安装成功后设置到`Path`环境变量中

    Path    C:\Python27
    
#### 3.2 安装Cocos2dx
目前我使用的是cocos2d-x-3.11.1，因为它是最后一个支持Cocos Studio的`csb`布局文件的版本，如果我们想使用cocos2dx提供的安装脚本来安装`cocos2d-x-3.11.1/setup.py`，则需要提前安装好Ant，并且将Ant设置到`ANT_ROOT`环境变量中

    ANT_ROOT C:\apache-ant-1.9.3\bin
    
我一般自己手动设置，只需要把`cocos2d-x-3.11.1/tools/cocos2d-console`，复制到虚拟机中，然后将它设置到`Path`环境变量中,就可以使用`cocos`命令来进行编译版本

    PATH C:\cocos2d-x-3.11.1\tools\cocos2d-console\bin
    
#### 3.3 安装NDK

由于cocos2d-x-3.11.1目前支持的NDK版本是`android-ndk-r10d`，所以我们从官方网站上下载对应位数的安装包，进行安装，记得安装的路径不能有空格，安装完后将它设置到环境变量`NDK_ROOT`中

    NDK_ROOT    C:\android-ndk-r10d

#### 3.4 设置Android SDK

设置Android SDK的路径，安装Android Studio的时候已经下载好了SDK，我们找到SDK的路径将它设置到环境变量`ANDROID_SDK_ROOT`中

    ANDROID_SDK_ROOT    C:\Program Files\Android\sdk

由于谷歌在新的SDK Tools中已经去掉了`android`命令，而`cocos`是使用`android`命令来进行编译的，所以要在网上下载一个旧版本`tools`来替换新版本的`sdk\tools`。

### 4. 编译版本

* 确保虚拟机里面有项目Gradle文件里面设置的SDK和Build Tools，Build Tools在`sdk\build-tools`目录下查看，SDK在`sdk\platforms`目录下查看。

* 一定要记得修改`cocos2d/cocos/platform/android/libcocos2dx/AndroidManifest.xml`里面的名称，因为这个名称在手机系统里面是可以看到的。





 