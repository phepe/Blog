---
title: Android Gradle和Gradle插件区别
date: 2018-07-25 12:35:58
categories: 
    - Android
tags:
    - Android
---

### 1. 概述

* Gradle是个构建系统，能够简化你的编译、打包、测试过程。
 
* Gradle Wrapper的作用是简化Gradle本身的安装、部署。不同版本的项目可能需要不同版本的Gradle，手工部署的话比较麻烦，而且可能产生冲突，所以需要Gradle Wrapper帮你搞定这些事情。Gradle Wrapper是Gradle项目的一部分。

* Android Plugin for Gradle是一堆适合Android开发的Gradle插件的集合，主要由Google的Android团队开发，Gradle不是Android的专属构建系统，但是有了Android Plugin for Gradle的话，你会发现使用Gradle构建Android项目尤其的简单。

<!-- more -->

### 2. 通过Android Studio理解Gradle

使用Android Studio打开项目，可以看到一个`gradle/wrapper`目录，其中有两个文件：`gradle-wrapper.jar`和`gradle-wrapper.properties`

* `gradle-wrapper.ja`r是Gradle Wrapper的主体功能包。在Android Studio安装过程中产生`gradle-wrapper.jar`，然后每次新建项目，会将gradle-wrapper.jar拷贝到你的项目的gradle/wrapper目录中。

* `gradle-wrapper.properties`文件主要指定了该项目需要什么版本的Gradle，从哪里下载该版本的Gradle，下载下来放到哪里。


Gradle对应版本下载完成之后，Gradle Wrapper的使命基本完成了，Gradle会读取`build.gradle`文件，该文件中指定了该项目需要的`Android Plugin for Gradle`版本是什么，从哪里下载该版本的`Android Plugin for Gradle`。


### 3. 总结

* `gradle-wrapper.properties`中配置的是的Gradle的版本.

* `build.gradle`中的依赖指定的是Gradle插件的版本.

Gradle与Gradle插件版本匹配，最好让Gradle和Gradle插件都更新到最新


| Plugin version | Required Gradle version |
| :----: | :----:|
| 1.0.0 - 1.1.3 | 2.2.1 - 2.3 |
| 1.2.0 - 1.3.1 | 2.2.1 - 2.9 |
| 1.5.0 | 2.2.1 - 2.13 |
| 2.0.0 - 2.1.2 | 2.10 - 2.13 |
| 2.1.3+ | 2.14.1+ |