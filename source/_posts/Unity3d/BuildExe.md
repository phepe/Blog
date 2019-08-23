---
title: Unity3d编译Exe版本
date: 2019-08-23 11:10:05
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本设置

* 在`Build Settings`里面将平台切换到PC，如果需要编译Debug版本就需要勾选`Development Build和Script Debugging`，这样当游戏崩溃的时候会有红色的提示，还有对应的日志文件，日志文件的路径一般在`C:\Users\***\AppData\LocalLow\CompanyName\ProductName\Player.log`,这个日志文件可以记录游戏的所有输出。
<!--more-->
* 点击`Player Settings` 设置公司名称、项目名称、分辨率。当分辨率设置不生效的时候，需要删除`PlayerPrefs`文件，或者通过代码来更新设置的分辨率，因为会默认保存上一次的分辨率，所以更新分辨率的时候不生效。

      void Start () {
          Screen.SetResolution(1920,1080,true);
      }
### 2.PlayerPrefs更新

在Windows下存储`PlayerPrefs`路径为`HKCU\Software\CompanyName\ProductName`,如果是通过Unity编辑器，则存储路径为`HKCU\Software\Unity\UnityEditor\CompanyName\ProductName`,因此如果想通过其他项目的编辑器来更新Exe文件的`PlayerPrefs`的数据，就需要将编译Exe的项目的名称设置为`Unity\UnityEditor\CompanyName`,然后在其他项目中使用这个项目的名称和Exe的存储路径进行匹配。这个主要是针对关卡编辑器独立出来的需要注意的。 每个平台的存储路径可以查看官方[文档](https://docs.unity3d.com/ScriptReference/PlayerPrefs.html)
