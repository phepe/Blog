---
title: Unity3d的声音系统
date: 2019-04-20 14:25:02
categories:
    - Unity3d
tags:
    - Game
---

### 1. 基本概念

在游戏中，一般存在两种音乐，一种是时间较长的背景音乐，一种是时间较短的音效，Unity3D支持下面几种音乐格式：

* AIFF：适用于较短的音乐文件可用作游戏打斗音效
* WAV：适用于较短的音乐文件可用作游戏打斗音效
* MP3：适用于较长的音乐文件可用作游戏背景音乐
* OGG：适用于较长的音乐文件可用作游戏背景音乐
<!--more-->

### 2. 音乐组件

Unity3D中对音乐进行了封装，总体来说，要播放音乐需要3个基本的组件：

* AudioListener：创建场景时在主Camera上就会带有这个组件，该组件只有一个功能，就是监听当前场景下的所有音效的播放并将这些音效输出，如果没有这个组件，则不会发出任何的声音。

* AudioSource：控制一个指定音乐播放的组件，可以通过属性设置来控制音乐的一些效果：
  * AudioClip：声音片段，还可以在代码中去动态的截取音乐文件；
  * Mute：是否静音；
  * Bypass Effects：是否打开音频特效；
  * Play On Awake：开机自动播放；
  * Loop：循环播放；
  * Volume：声音大小，取值范围0.0 到 1.0；
  * Pitch：播放速度，取值范围在 -3 到 3 之间 设置1 为正常播放，小于1 为减慢播放 大于1为加速播放；

* AudioClip：当我们把一个音乐导入到Unity3D中，这个音乐文件就会变成一个AudioClip对象，即我们可以直接将其拖拽到AudioSource的AudioClip属性中，也可以通过Resources或AssetBundle进行加载，加载出来的对象类型就是AudioClip。

### 3. Play和PlayOneShot的区别

Play方法可以和其它方法配合使用，比如Pause（暂停）和Stop（停止），播放的是clip属性对应的Audio Clip对象，同一时刻只会有一个clip音乐进行播放。如果要同时使用Play方法播放两个音乐就需要再添加一个Audio Source对象了。

而PlayOneShot是指马上播放一个音乐且只播放一次，同时Pause和Stop对其无效，如果我们调用该方法播放多次音乐，则多个音乐会同时被播放出来。

Play方法适合播放背景音乐，因为背景音乐同一时刻只会有一个再播放，而且还需要可以控制其播放和暂停等。

PlayOneShot方法适合播放音效，因为音效一般只会播放一次且不需要其它的控制，同时多个音效播放时是可以共存的，即可以听到多个声音。

### 4. 其他

播放背景音乐的时候最好将音乐的`Load Typ`e设置为`Streaming`，可使内存使用量减至最低，但相对的会占用更多CPU资源和I/O吞吐量。

Unity为啥要把音乐播放拆分成这3个组件呢？其中最重要的就是实现3D音效效果。

如果我们将Audio Listener看作一双耳朵的话就可以很好的理解什么是3D音效效果了，Unity会根据Audio Listener对象所在的GameObject和Audio Source所在的GameObject判断一下距离和位置来模拟真实世界中的音量近大远小的效果。
