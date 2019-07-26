---
title: Unity3d的粒子系统
date: 2019-04-20 14:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

Unity的粒子系统既健壮又功能丰富，粒子系统通常在预定义空间内的随机位置发射粒子，其可具有类似球形或锥形的形状。 系统确定粒子本身的寿命，当寿命到期时，系统会破坏粒子，具体的表现效果由几部分组成：速度多大、持续多久、颜色怎么变化、是否需要使用 Sprites 来做、是否需要与光照交互。
<!--more-->

### 2. Particle System主要参数

[参考文章1](https://blog.csdn.net/NCZ9_/article/details/84191694)

[参考文章2](https://www.jianshu.com/p/424dc71950dd)

[参考文章3](https://www.jianshu.com/p/e6551726e1ad)

ParticleSystem Initial（初始化模块）此模块为固有模块，无法将其删除或禁用。该模块定义了粒子初始化时的一系列基本参数。

Emission（发射模块）在粒子的发射时间内，可实现在某个特定的时间生成大量粒子的效果，这对于模拟爆炸等需要产生大量粒子的情形非常有用。

Shape（形状模块）定义粒子发射器的形状，可提供沿着该形状表面法线或随机方向的初始力，并控制粒子的发射位置及方向。

Velocity over Lifetime（生命周期内速度）控制生命周期内每一个粒子的速度，对于那些物理行为复杂的粒子，效果更明显，但对于那些具有简单视觉行为效果的粒子（如烟雾飘散效果）以及与物理世界几乎没有互动行为的粒子，此模块的作用就不明显了。

Limit Velocity over Lifetime（生命周期内速度限制）控制粒子在生命周期内的速度限制及速度衰减，可以模拟类似拖动的效果。若粒子的速度超过设置的限定值，则粒子速度值会被锁定到该限制值。

Inherit Velocity（继承速度）指定当粒子出生时，发射器速度是否继承为一次性、是否始终使用当前发射器速度，还是在粒子出生时使用发射器速度。

Force over Lifetime（生命周期内受力）控制粒子在其生命周期内的受力情况。

Color over Lifetime（生命周期内颜色）控制每一个粒子在其生命周期内的颜色变化。

Color by Speed（颜色的速度控制）让每个粒子的颜色依照其自身的速度变化而变化。

Size over Lifetime（生命周期内大小）控制每一个粒子在其生命周期内的大小变化。

Size by Speed（粒子大小的速度控制）可让每个粒子的大小依照其自身的速度变化而变化。

Rotation over Lifetime（生命周期内旋转）控制每一个粒子在其生命周期内的旋转速度变化。

Rotation by Speed（旋转的速度控制）可让每个粒子的旋转速度依照其自身的速度变化而变化。

External Forces（外部作用力）可控制风域的倍增系数。

Noise（噪音）

Collision（碰撞）为粒子系统建立碰撞效果。

Triggers（触发器）

Sub Emitters（子发射器）粒子的子发射器。可使粒子在出生、消亡、碰撞等三个时刻生成其他的粒子。

Texture Sheet Animation（序列帧动画纹理模块）可对粒子在其生命周期内的UV坐标产生变化，生成粒子的UV动画。可以将纹理划分成网格，在每一格存放动画的每一帧。同时也可以将纹理划分为几行，每一行是一个独立的动画。

Lights（光线）

Trails（拖尾）

Custom Data(自定义数据)

Renderer（渲染器）显示粒子系统渲染相关的属性。即使此模块被添加或移除，也不影响粒子的其他属性。

### 2. 制作粒子效果

一般需要需要将Materail拖动到Renderer里面的Material，使用的最多的就是Particles/Alpha Blended的材质。
