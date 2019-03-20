---
title: Unity3d 反编译
date: 2019-02-20 18:25:00
categories: 
    - Unity3d
tags:
    - Android
---

### 1. 反编译 APK
#### 1.1 Unity3d Android Apk解压后文件介绍


| 目录 | 介绍 |
| :----: | :----:|
| assets | 原始资源文件夹，对应着Android工程的assets文件夹，一般用于存放原始的图片、txt、css等资源文件 |
| lib | 存放应用需要的引用第三方SDK的so库。比如一些底层实现的图片处理、音视频处理等。这是根据不同CPU 型号而划分的，如 ARM，ARM-v7a，x86等 |
| META-INF | 保存apk签名信息，保证apk的完整性和安全性 |
| res | 资源文件夹，其中的资源文件包括了布局(layout)，常量值(values)，颜色值(colors)，尺寸值(dimens)，字符串(strings)，自定义样式(styles)等 |
| AndroidManifest.xml | 全局配置文件，里面包含了版本信息、activity、broadcasts等基本配置。不过这里的是二进制的xml文件，无法直接查看，需要反编译后才能查看。 |
| classes.dex | 安卓代码核心部分，dex是在Dalvik虚拟机上可以执行的文件。如果有classes.dex和classes2.dex两个文件，说明工程的方法数较多，进行了拆分 |
| resources.arsc | 记录资源文件和资源id的映射关系 |

  <!--more-->


 `assets\bin\Data`，Unity3d的相关资源和代码都在这个里面。
 
  * `Managed`下放的是`dll`，游戏源码被编译成`Assembly-CSharp
  .dll`（还有其他代码放到了`*-first-pass.dll`之类的代码，但不是主要的），如果没加密直接引用对应的`dll`到`monodeveloper`就看以考到源代码。
  
  * 判断一个游戏是不是用Unity3d做的 可以查看`assets\bin\Data`目录下是否有`unity default 
  resources`文件。
  
  * `leve*`表示游戏场景，一般有几个就表示游戏用了几个场景。
  
  * `sharedassets*`和场景对应。`*.asset`或者`*.assets.split0` 是游戏资源
  
  直接查看apk压缩文件，只有assets可以直接查看

#### 1.2 Android Java代码的反编译

  * 要将apk里的dex文件转换成Jar包，再通过工具查看编译前的java源文件
  
  * 过工具查看apk里对应的AndroidManifest.xml、resources.arsc、res各布局文件等二进制文件
  
  需要的工具主要有以下几个:
  
   * dex2jar:将dex文件转换成Jar包 [下载地址](https://sourceforge.net/projects/dex2jar/files/)
   
   * jd-gui:将Jar包文件反编译成java源文件 [下载地址](http://java-decompiler.github.io/)
   
   * apktool:查看二进制文件 [下载地址](https://ibotpeaches.github.io/Apktool/install/)
   
  Mac上主要步骤：
  
  * 使用dex2jar将dex文件转换成Jar包，关键需要用到的文件是`d2j-dex2jar.sh`和`d2j_invoke.sh`，需要可执行权限,命令结束后会在当前目录下生成`classed-dex2jar
   .jar`文件。（如果apk的方法数超过了65535，会生成多个dex文件，反编译的话需要对这多个dex文件均进行转换Jar包处理）。
    
                ./d2j-dex2jar/d2j-dex2jar.sh classes.dex
        
  * 使用jd-gui将Jar包文件反编译成java源文件，打开图形化界面，将上一个步骤生成的`classes-dex2jar.jar`直接拖动进入界面中，就可以看到反编译之后的源码结构了(这里有时候会看到诸如a、b等字母标示的包名、类名或者方法名，这是由于在某些apk打包的时候进行代码混淆导致的。使用反编译工具只能看到混淆之后的代码结构，真正未混淆前的源码还需要结合mapping.txt文件进行分析。mapping.txt文件里即标注出了代码混淆前后的文件名称对应关系)。
  
  * 使用apktool工具查看apk里的二进制文件，命令执行完后，会在当前目录下新增yourApkName文件夹，其中可以看到可读的`AndroidManifest.xml`、`res`目录下的各布局文件、`assets`文件夹等。`original`文件夹是原始的`AndroidManifest.xml`文件，`res`文件夹是反编译出来的资源，`smali`文件夹是反编译出来的代码。注意，`smali`是有点类似于汇编的语法，是Android虚拟机所使用的寄存器语言
    
                    java -jar apktool.jar d yourApkName.apk
                    
  * `Android-classyshark`反编译工具 [下载地址](https://github.com/google/android-classyshark/releases)，
  下载下来之后是一个可执行的jar文件，在终端执行java -jar classyshark.jar即可打开图形化界面。在打开的图形操作界面中拖入待目标apk，即可展示出反编译之后的结果。
  
  * Android Studio 只需要将目标apk拖入到 Build --> APK Analyzer中就可以将APK全部反编译出来。同时可以为APK瘦身，分析APK各个部分的大小。
 
#### 1.3 提取Unity游戏资源和脚本

  * UnityStudio ，选中`assets\bin\Data`该目录，游戏图片，声音都可以很方便的查看，导出，就是不支持查看代码 。其中`Scene Hierarchy`可以查看游戏里面场景的分布情况，而`Asset 
  List`可以查看资源[下载地址](https://github
  .com/Perfare/AssetStudio/releases)

  * .NET Reflector(.net反编译工具)，查看代码，需要破解，把`Assembly-CSharp.dll`直接拖进去，就可以看的所有的代码，类名，方法名，方法体，一清二楚。右键点击就可以导出。[下载地址](https://www
  .jb51.net/softs/629309.html#downintro2)
  
  * DevXUnity-Unpacker Magic Tools，可以将APK转化成Unity工程，免费版本可以查看代码和资源。[下载地址](http://devxdevelopment.com/)
 
 






 













 