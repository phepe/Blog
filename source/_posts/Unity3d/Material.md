---
title: Unity3d的Shader和Material
date: 2019-04-24 19:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

Shader（着色器）：实际上就是一小段程序，它负责将输入的Mesh（网格）以指定的方式和输入的贴图或者颜色等组合作用，然后输出。绘图单元可以依据这个输出来将图像绘制到屏幕上。输入的贴图或者颜色等，加上对应的Shader，以及对Shader的特定的参数设置，将这些内容（Shader及输入参数）打包存储在一起，得到的就是一个Material（材质）。之后，我们便可以将材质赋予合适的renderer（渲染器）来进行渲染（输出）了。

输入的贴图或者颜色等，加上对应的Shader，以及对Shader的特定的参数设置，将这些内容（Shader及输入参数）打包存储在一起，得到的就是一个Material（材质）。之后，我们便可以将材质赋予合适的renderer（渲染器）来进行渲染（输出）了。换句话说，Shader定义了我们要用哪些参数来定义一个Material，是一个Material的模板。当我们把模板中需要的参数，例如Color，Texture等填充上去之后，我们就得到了我们想要的Material

<!--more-->

### 2. 创建Material和Shader

Unity中材质Material：指色彩Main Color，纹理Texture，着色器shader（光滑度，透明度，反射率，折射率，发光度）等。因为预览方式是个球体，有时又被称作材质球。

Unity中创建材质Material和Shader：在Project项目面板中创建材质。

Shader不能直接拖拽到游戏物体上，需要我们先新建一个Material，然后把Shader赋予给Material，再把Material赋予给游戏物体。有些Shader还需要把对应的C#脚本赋予给游戏物体。


Alpha  Blending主要用于实现半透明效果，类似于玻璃
