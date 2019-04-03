---
title: Unity3d学校笔记
date: 2019-03-20 14:25:02
categories: 
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

* Unity中的一个技巧是，创建一个空的游戏对象，并将其作为其它对象的文件夹，它将简化你场景的层次结构。注意最好将空对象的坐标设置为(0, 0, 0)，将这些空对象视为纯逻辑对象。
* 2D和3D使用的物理引擎是不一样的Box2D/PhysX。 
* 物理相关的处理尽量在`FixedUpdate`里进行。
* UGUI: Unity5.X后(其实是Unity4.6以后)，Unity找到NGUI的作者，用了一年开发了UGUI，变成内置于Unity中的包，官方主推。
<!--more-->

### 2. UGUI与NGUI的对比差别
#### 2.1 主要概率的对比


| | NGUI | UGUI |
| :----: | :----: | :----:|
| 根节点 | UIRoot | Canvas |
| UI面板 | Panel | Canvas |
| UI容器 | uiWidget | Panel |
| 事件交互 | Collider | EventSystem |
| 锚点 | Anchor | RectTransform Anchor |
| 图片 | Sprite | Image |
| 文字 | Label | Text |
| 单张贴图 | Texture | RawImage |
| UI相机 | camera + UICamera | camera + EventSystem |


#### 2.2 主要组件的对比

| | NGUI | UGUI |
| :----: | :----: | :----:|
| Button | ✓ | ✓ |
| Toggle | ✓ | ✓ |
| Scroll Bar | ✓ | ✓ |
| Progress Bar | ✓ | ✓ |
| Input Field | ✓ | ✓ |
| Popup List | ✓ | x |
| Localization | ✓ | x |
| Play Sound | ✓ | x |
| Scroll View | ✓ | ✓ |

总体来说，对于基本的UI元素， NGUI与UGUI都有；在组件的丰富度上，UGUI略逊于NGUI。例如，UGUI没有Localization、Play Sound组件；NGUI的Label是支持静态图文混排跟文字URL超链接的，而UGUI不支持。所以使用UGUI的时候需要自行开发以上缺失的组件及功能。


### 3. UGUI 的Canvas


UGUI的UI根目录为canvas（画布），是一个控制一组被渲染UI元素的组件,所有的UI元素必须是Canvas的子物体,
在Hierarchy中点击Create——UI——Text，系统会自动帮我们创建一个父Canvas节点和EventSystem节点。EventSystem节点，用来专门接受管理事件。

Canvas Canvas属性一览：
* Render Mode 定义UI如何渲染于屏幕之上，可选项包括：
    * Screen Space - Overlay
    * Screen Space - Camera
    * World Space
* Pixel Perfect 是否抗锯齿，仅适用于Screen Space Overlay/Camera模式。
* Render Camera 定义用于渲染UI的摄像机，仅适用于Screen Space - Camera模式。
* Plane Distance 定义UI平面摆放在摄像机的前方距离，仅适用于Screen Space - Camera模式。
* Event Camera 定义处理UI事件的摄像机，仅适用于World Space模式。

通常一个Scene只需一个Canvas，但多个Canvas也是支持的。为了优化目的，也支持Canvas嵌套，也即将某个Canvas作为另一个Canvas子节点。子Canvas跟父Canvas的Rander Mode保持一致。

Canvas组件三种模式的区别： 

* Screen Space - Overlay

    此模式下，Canvas会被缩放以适应屏幕，然后直接渲染到屏幕上，无需任何摄像机（即使当前Scene中没有任何摄像机）。若屏幕尺寸或者分辨率变更了，则UI会自动缩放来适应。UI会覆盖所有其他摄像机所见的画面。

* Screen Space - Camera

    此模式下，Canvas会渲染于摄像机前面指定距离的一个平面上。UI在屏幕上的大小并不随此距离变化而变化，因为UI总是被缩放来准确适配Camera Frustum。若Camera Frustum或屏幕尺寸或分辨率发生变化，UI会自动缩放来适应。Scene中任何比UI平面距离摄像机更近的3D物件，都将先于UI被渲染。相反位于UI平面后面的物件就会被遮挡。

* World Space

    此模式下，UI被看作是Scene中的一个平面物件。不像Screen Space - Camera模式，此时UI平面并不需要面向摄像机，而是可以面向任意一个方向。通过RectTransform可以设置Canvas尺寸，但他的屏幕大小将取决于摄像机的视角以及距离。其他场景中物件可能位于Canvas背面，或穿透Canvas，或位于Canvas前面。









 