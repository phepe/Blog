---
title: Unity3d的Canvas
date: 2019-04-20 14:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

使用UGUI来进行UI开发，离不开Canvas组件，所有的 UI 元素，要么自己包含 Canvas 组件，要么是 Canvas 组件所在 GameObject 的子节点。
<!--more-->

### 2. Canvas主要参数

* Render Mode
  * Screen Space - Overlay：屏幕控件-覆盖模式，画布会填满整个屏幕空间，可以理解为Unity为你自动设置好了UICamera，而且这个相机的Depth值是大于100的（相机能设置的最大Depth值为100），所以永远显示在最前面。此模式UICamera的Z值应该是-1000，所以Z值只要大于-1000并在UICamera的正交投影范围内，就有可能显示的UI界面上。
    * Pixel Perfect：UI元素精确到像素对齐，使UI元素像素对应，效果就是边缘清晰不模糊

    * Sort Order：指示画布的深度，多个Canvas时可以调整其显示顺序

    * Target Display：目标显示器，Unity多开时选择显示器用

  * Screen Space - Camera：屏幕空间-摄影机模式，画布也是填满整个屏幕空间，如果屏幕尺寸改变，画布也会自动改变尺寸来匹配屏幕，此模式我们需要一个Camera，这个相机的作用就是把它所投射获取到的界面当做UI界面。一般情况下，UI界面只是一个二维平面，所以把相机的投影设置为Orthographic，即正交投影，`Culling Mask`设置为UI，表示只显示跟UI层相关的信息，接着再调整一下相机的Size，因为相机是锥形的，让他的大小与Canvas的保持一致，最后再设置一下相机的Z值，保证Canvas在相机之前就搞定了。改模式下面 UI 并不一定能渲染在 3d 元素之上。
    * Render Camera：渲染摄像机
    * Plane Distance：Canvas与Camera的距离
    * Sorting Layer： 用来指示画布的深度，画布所使用的Sorting Layer越排在下面，显示的优先级也就越高，针对多个画布显示
    * Order in Layer：在相同的Sort Layer下的画布显示先后顺序。数字越高，显示的优先级也就越高。多个 Canvas 具有相同的 Sorting Layer 时，根据 Order in Layer 来确定显示优先级。

  * World Space：把UI当做三维物体来处理，跟随人物移动的血条或者名称。

Canvas渲染优先级： Sort Order -> Sorting Layer -> Order in Layer

相同的 Screen Space - Overlay：显示优先级由 Sort Order 确定

相同的 Screen Space - Camera：显示优先级由 Sorting Layer 和 Order in Layer 确定

不同模式的 Canvas之间，Screen Space - Overlay 的 Canvas 永远显示在最前面，Screen Space - Camera 和 World 的显示关系决定于 World Canvas 距离摄像机的位置以及 Screen Space - Camera Canvas 的 Plane Distance


  | 渲染模式 | 画布对应屏幕 | 摄像机 | 像素对应 | 合适类型 |
  | :----: | :----: | :----:| :----:| :----:|
  | Screen Space-Overlay | 是 | 不需要 | 可选 | 2D UI |
  | Screen Space-Camera | 是 | 需要 | 可选 | 2D UI |
  | World Space | 否 | 需要 | 不可选 | 3D UI |



### 3. Canvas Scaler 组件

Canvas Scaler是Unity UI系统中，控制UI元素的总体大小和像素密度的组件，Canvas Scaler的缩放比例影响着Canvas下的元素，包含字体大小和图像边界。

* UI Scale Mode：UI缩放模式
  * Constant Pixel Size：保持像素尺寸，忽略屏幕尺寸,UI在任何分辨率下都不会进行缩放拉伸，只有通过改变Scale Factor才会进行缩拉，因此不推荐使用该模式(而这种模式的优点就是你可以通过写自适应算法来改变Scale Factor的值，代替unity的自适应算法)

    * Scale Factor: 对Canvas下的UI整体缩放

    * Reference Pixels Per Unit: 每单位像素量(sprite默认每单位像素量为100)

  * Scale With Screen Size： 根据屏幕宽度缩放UI尺寸,相当于使用unity的自适应算法，此时unity会根据屏幕分辨率自动调节Scale Factor的值。在做自适应时，一般要先选择一种比较主流的分辨率(即比较多的机型都采用这种分辨率)进行UI的设计，例如采用1024x576，在这里就是设置Reference Resolution的值了。

    * Reference Resolution: 参考分辨率(一般设为1280x720或者1920x1080)

    * Screen Match Mode: 屏幕匹配模式

      * Match Width or Height: 匹配屏幕宽度或者高度(通过Match值调节匹配权重),当值为0即处于Width那端时，表示屏幕高度对于UI大小完全没有任何影响，只有宽度会对UI大小产生影响。例如设置屏幕为800*600，然后改变为800*300，屏幕高度变小了，但UI并没有进行缩拉；同理当值为1即处于Height那端时，表示屏幕宽度对于UI大小完全没有任何影响，只有高度会对UI大小产生影响。当值为0.5的时候，对宽哥进行加权缩放。屏幕自适应常用方式 ！！！

      * Expand: 横向或纵向展开画布区域，所以画布的大小永远不会小于参考

      * Shrink: 在水平或垂直方向上收缩画布区域，所以画布的大小永远不会比参考大

      * Reference Pixels Per Unit: 如果精灵具有‘Pixels Per Unit’设置，则精灵中的一个像素将覆盖UI中的一个单位

    * Constant Physical Size：保持UI的物理尺寸，忽略屏幕尺寸和分辨率

      * hysical Unit: 用于指定位置和大小的物理单位。 Centimeters(厘米)、Millimeters(毫米)、Inches(英寸)、Points(点)

      *  Fallback Screen DPI: 如果屏幕DPI未知,则用此值替代

      * Default Sprite DPI: 每英寸像素用于有‘Pixels Per Unit’设置的精灵，与 ‘Reference Pixels Per Unit’相匹配。

      * Default Sprite DPI: 每英寸像素用于有‘Pixels Per Unit’设置的精灵，与 ‘Reference Pixels Per Unit’相匹配。

一般来说，比较不错的设置就是：`Canvas Scaler` 选择 `Scale With Screen Size`
`Screen Match Mode` 选择 `Match Width Or Height`，比例设为1，即只和高度进行适配。

注意 Math 设置需要根据实际需要 分情况而定

* 横屏：屏幕高度确定是1080，宽不确定，可能是双排，这是屏幕适配 with=0

* 竖屏：屏幕宽度确定比如800，高度不确定，这时屏幕适配hight=1

[参考文章](https://gameinstitute.qq.com/community/detail/127148)

### 4. Graphic Raycaster组件

关于UI射线检测的设置

* Ignore Reversed Graphics：是否忽略反转图片的检测。

* Blocking Objects：在Canvas前面，可以遮挡射线检测的物体。

* Blocking Mask：遮挡射线检测的层级。
