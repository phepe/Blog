---
title: Unity3d的Serialize功能
date: 2019-04-20 14:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

Unity3D 中提供了非常方便的功能可以帮助用户将 成员变量 在Inspector中显示，并且定义Serialize关系，Unity会自动为Public变量做序列化，序列化的意思是说再次读取Unity时序列化的变量是有值的，不需要你再次去赋值，因为它已经被保存下来，保存游戏的时候，这些值就会被保存成二进制文件。

**已经被序列化，但是没有用HideInInspector标记的值，会被显示在面板上。**

<!--more-->

### 2. SerializeField和Serializable

* Public变量：在没有加入任何Attribute的前提下，public变量是默认被视为可以被Serialize的。

* SerializeField：将原本不会被序列化的私有变量和保护变量可以序列化，这么他们在下次读取时，就是你上次赋值的值。在Unity最新的UI系统中，UI属性上方全部添加[SerializeField] ，如下所示：

      [SerializeField]
      private Button btn1;
* Serializable: 有时候我们会自定义一些单独的class/struct, 由于这些类并没有从 MonoBehavior 派生所以默认并不被Unity3D识别为可以Serialize的结构。自然也就不会在Inspector中显示，我们可以通过添加 [System.Serializable]这个Attribute使Unity3D检测并注册这些类为可Serialize的类型。具体做法如下：

    [System.Serializable]
    public class FooBar {
        public int foo = 5;
        public int bar = 10;
    }

* 如果变量加入了readonly, const, static等修饰符，无论他的serialize设置如何，都将不会进行serialize

* 默认情况下，protected, private, internal变量将不会被serialize.

* 有时候我们需要定义一些public变量方便操作，但是又不希望这些变量保留。这个时候可以利用[System.NonSerialized]来完成这个操作

* Unity3D可以对List<T>进行序列化显示，但是不能serialize Dictionary<T,K>。通常我们会通过Serialize一份List<T>，然后在 Awake中初始化Dictionary的方法来完成Dictionary的serialize操作。

* 变量在Inspector中会根据变量的大写字母来隔开来显示，并且会将首字母强制大写的方式显示。

[参考文章] (https://www.cnblogs.com/zhaoqingqing/p/3995304.html)
