---
title: Unity3d的动画
date: 2019-04-25 19:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

Unity的Mecanim动画系统是非常强大的，而且作为Unity推荐的动画系统，其未来会完全代替老的一套动画系统，即Legacy动画系统。Unity3D Mecanim动画系统是Unity4.x之后新添加的系统。

<!--more-->

### 2. 创建动画

创建动画第一张方式：
* 添加 Animator 组件
* 创建一个 Animator Controller，赋值给Animator组件
* 双击打开Animator，新建状态
* 创建Animation Clip,赋值给新状态的Motion
* 编辑Animation Clip实现动画

创建动画的另外一种方式：

* 新建一个Animation
* 将Animation拖拽到游戏对象上，会自动创建好Animator
* 编辑动画

### 3.编辑动画

* 在`Animator Controller`里面添加`Make Transition`来设置各种动画状态的转换，通过添加状态控制参数来通知`AnimaState`
* 编辑切换状态的条件,编辑动画是否退出
* 灵活使用`Animator Override Controller`来重用`Animator Controller`的转换逻辑
