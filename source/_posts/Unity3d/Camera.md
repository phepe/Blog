---
title: Unity3d的Camera
date: 2019-04-24 19:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

Camera是玩家观察世界的装置，屏幕空间点按像素定义，屏幕的左下为（0,0）；右上是（pixelwidth，pixelHeight）。在Unity中创建一个Camera后，除了默认带的Transform组件外，还会带上Audio Lisenter组件，如果场景中没有这个组件，将听不到任何声音。
<!--more-->

### 2. Camera组件属性

* `Clear Flags`： 清除标记，决定屏幕中哪些部分被清楚，一般用于多台摄像机来描述不同对象的情况（人和枪）。每个相机在渲染时会存储颜色和深度信息，屏幕未绘制的部分是空的，默认情况下会显示天空盒，可以设置清除标记来清除缓冲区的信息。
  * Solid Color： 纯色，显示此处的默认背景
  * Skybox：天空盒，在屏幕空白处显示当前摄像机的天空盒
  * Depth only：仅深度，拥有对象不被检测（人物拿枪）
  * Don't Clear：不清除，所有的结果都会叠加在下一帧，造成涂片效果
* Background：没有Skybox的时候显示纯色时设置的背景
* Culling Mask：剔除遮罩，根据对象所指定的层来控制渲染
* Projection：投影方式
  * Orthographic： 正交
  * Perspective： 透视
* Size：相机的视口的大小，相机的广角
* Clipping Planes：剪裁平面，摄像机的渲染范围，Near为最近点，Far为最远点
* ViewPort Rect：视图矩形，用四个数值来控制摄像机的视图在屏幕中的位置和大小，使用的是屏幕坐标系，数值在0~1之间，X（水平位置起点）、Y（垂直位置起点）、W（宽度）、H（高度）
* Depth：深度，用于控制摄像机的渲染顺序
* Rendering Path：渲染路径，一般使用玩家设定
* Target Texture：目标纹理
* Occlusion Culling：是否剔除物体背向摄像机的部分
* HDR：高动态光照渲染，能让场景变的更真实，光照的变化不会显得太突兀
* MSAA：是否启用MSAA渲染目标
* Allow Dynamic Resolution：动态分辨率缩放
* Target Dilsplay：设置摄像机的目标显示
