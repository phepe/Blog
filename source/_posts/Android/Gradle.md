---
title: Android Gradle
date: 2018-07-25 12:35:58
categories: 
    - Android
tags:
    - Gradle
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


### 4. implement、api 和compile区别

Android studio升级到3.0以上后，`com.android.tools.build:gradle`也升级到了`3.0.0`，在`3.0.0`中使用了最新的Gralde 4.0 作为gradle 的编译版本，该版本gradle编译速度有所提升，完全支持Java8，Kotlin插件默认是安装的。在3.0版本中，`compile`指令被标注为过时方法，而新增了两个依赖指令，`implementation`和`api`。

`compile`指令可以依赖传递，依赖对`app Module` 是可见的

`api`指令完全等同于`compile`指令，没区别，你将所有的`compile`改成`api`，完全没有错。使用`api`声明依赖，如果依赖发生变化，所有访问到此依赖的模块都需要重新编译。因此它会增加模块的编译时间。

`implementation`指令不可以依赖传递，依赖对`app Module`是不可见的，对于使用了该命令编译的依赖，对该项目有依赖的项目将无法访问到使用该命令编译的依赖中的任何程序，也就是将该依赖隐藏在内部，而不对外部公开，这样的好处是编译速度会加快。
Google推荐使用implementation的方式去依赖，如果你需要提供给外部访问，那么就使用api依赖即可。

implementation的好处：

 * 依赖关系不会泄漏到消费者的编译类路径中，所以永远不会意外地依赖于传递依赖项 
 * 由于减少的类路径大小编译更快
 * 当实现依赖关系发生变化时，重新编译会更少：消费者不需要重新编译
 * cleaner发布：当与新的`maven-publish`插件结合使用时，Java库会生成POM文件，这些文件可以精确地区分编译库所需的内容和运行时使用库所需的内容（换句话说，不要混合编译library本身所需的东西，以及编译`library`所需的东西）。

 
大部分情况下都应该使用`implementation`替换`compile`，只有在一些库模块才考虑使用`api`替换`compile`。
