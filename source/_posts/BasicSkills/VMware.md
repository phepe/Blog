---
title: WMware coco2dx环境安装
date: 2018-08-06 16:16:10
categories: 
    - Basic Skills
tags:
    - WMware
---

### 1. 安装虚拟机

如果想要兼容新版本的Android Studio，最好下载64位的Win7系统，如果想轻松安装，就要下载系统小于4G的，不然会报错 `Directory "EZBOOT" not found`，VMware Fusion的版本使用的是10.0.1。然后按照以下步骤创建虚拟机：

* 新建Win7虚拟机，选择下载下来的ISO镜像，内存建议4G，硬盘40G，操作系统选择win7 64位。
* 进入电脑的BIOS开启`Intel virtualization technology`,允许CPU虚拟化64位，开启过的就不用再次开启了(Windows下进入BIOS可以通过电源里面的选项)。
* 将CD/DVD设置成IDE，不然会报错`nor find file win7.gho`。
* 使用PQ/PM设置系统分区，类型选择NTFS，然后进阶，设置为作用
* 从CD/DVD启动，选择安装到第一分区，进行安装。
* 安装过程中会弹出一个对话框，选择否，否则会卡住。
* 安装完成后安装VMware Tools，点击安装后如果没有弹出安装界面，需要手动在我的电脑里面找到`setup.exe`文件，点击安装。
* 32位的操作系统需要自己去安装32位的[JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
Android Studio安装自带的是64位的，安装好后要配置Java的环境变量，使用`java -version` 检测



<!-- more -->

### 2. 安装Android开发环境

需要准备Android Studio和JDK安装包:

* 在谷歌的官方网站上下载Android Studio，进行安装，最好不要安装在有空格的目录下，因为Cocos2dx识别环境变量的时候不能有空格，打开Android Studio安装SDK，选择自定义安装，这样就可以选择SDK安装的路径。

* 如果Android可以顺利打开，则不需要再去下载JDK，只需要设置一下环境变量就行。

        Path    C:\Android\Android Studio\jre\bin;

* 安装完成后如果提示没有Java环境，则需要去Orecal的Java官方网站上下载JDK进行安装，然后将JDK的路径设置到`Path`环境变量中。如果没有Path环境变量，则新建一个。

        Path    C:\Program Files\Java\jdk1.7.0_67\bin;

### 3. 安装Cocos2dx开发环境

#### 3.1 安装Python

在Python[官方网站](https://www.python.org/downloads)下载最新的Python2.7版本，注意要下载对应系统位数的版本，然后进行安装，安装成功后设置到`Path`环境变量中

    Path    C:\Python27
    
#### 3.2 安装Cocos2dx
目前我使用的是cocos2d-x-3.11.1，因为它是最后一个支持Cocos Studio的`csb`布局文件的版本，如果我们想使用cocos2dx提供的安装脚本来安装`cocos2d-x-3.11.1/setup.py`，则需要提前安装好Ant，并且将Ant设置到`ANT_ROOT`环境变量中

    ANT_ROOT C:\apache-ant-1.9.3\bin
    
我一般自己手动设置，只需要把`cocos2d-x-3.11.1/tools/cocos2d-console`，复制到虚拟机中，然后将它设置到`Path`环境变量中,就可以使用`cocos`命令来进行编译版本

    PATH C:\cocos2d-x-3.11.1\tools\cocos2d-console\bin
    
#### 3.3 安装NDK

由于cocos2d-x-3.11.1目前支持的NDK版本是`android-ndk-r10d`，所以我们从官方网站上下载对应位数的安装包，进行安装，记得安装的路径不能有空格，安装完后将它设置到环境变量`NDK_ROOT`中

    NDK_ROOT    C:\android-ndk-r10d
    
NDK下载地址:

[Windows32-bit](http://dl.google.com/android/ndk/android-ndk-r10d-windows-x86.exe)
[Windows64-bit](http://dl.google.com/android/ndk/android-ndk-r10d-windows-x86_64.exe)
[MacOS X 64-bit](http://dl.google.com/android/ndk/android-ndk-r10d-darwin-x86_64.bin)
[MacOS X 32-bit](http://dl.google.com/android/ndk/android-ndk-r10d-darwin-x86.bin)

#### 3.4 设置Android SDK

设置Android SDK的路径，安装Android Studio的时候已经下载好了SDK，我们找到SDK的路径将它设置到环境变量`ANDROID_SDK_ROOT`中

    ANDROID_SDK_ROOT    C:\Android\sdk

由于谷歌在新的SDK Tools中已经去掉了`android`命令，而`cocos`是使用`android`命令来进行编译的，所以要在网上下载一个旧版本`tools`来替换新版本的`sdk\tools`。
[Windows下载地址](https://dl.google.com/android/repository/tools_r25.2.5-windows.zip)

[Mac下载地址](https://dl.google.com/android/repository/tools_r25.2.5-macosx.zip)

#### 3.5 最后的环境变量

    Path    C:\Python27;C:\Android\Android Studio\jre\bin;C:\cocos2d-console\bin
    ANDROID_SDK_ROOT    C:\Android\sdk
    NDK_ROOT    C:\Android\android-ndk-r10d

### 4. 编译版本

* 确保虚拟机里面有项目Gradle文件里面设置的SDK和Build Tools，Build Tools在`sdk\build-tools`目录下查看，SDK在`sdk\platforms`目录下查看。

* 一定要记得修改`cocos2d/cocos/platform/android/libcocos2dx/AndroidManifest.xml`里面的名称，因为这个名称在手机系统里面是可以看到的。


### 5. 其他

安装[shadowsocks](https://github.com/shadowsocks/shadowsocks-windows/releases)翻墙，
需要安装 [.NET Framework 4.6.2](https://www.microsoft.com/zh-CN/download/details.aspx?id=53344)，然后如果无法写入注册表，需要保留360相关的软件,
卸载后会导致用不了Shadowsocks,最好在安装完系统后不要卸载任何软件，确保shadowsocks可以用之后在卸载，卸载后再次重启确定SS是否可用。然后在安装开发环境。






 