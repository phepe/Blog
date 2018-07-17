---
title: RecyclerView使用详解
date: 2018-07-15 16:15:50
categories: 
    - Android
tags:
    - Android
---

> RecyclerView是谷歌官方出的一个用于在有限的窗口中展示大量数据集的新控件，可以用来代替传统的ListView，更加强大和灵活。

### 1. RecyclerView基础用法
#### 1.1 概述

RecyclerView提供了一种插拔式的体验，高度的解耦，异常的灵活，通过设置LayoutManager、ItemDecoration、ItemAnimator实现令人瞠目的效果。

* 设置布局管理器以控制Item的布局方式，横向、竖向以及瀑布流方式。
* 可设置Item操作的动画（删除或者添加等)
* 可设置Item的间隔样式（可绘制）

但是关于Item的点击和长按事件，需要用户自己去实现。
<!-- more -->

在使用RecyclerView时候，必须指定一个适配器Adapter和一个布局管理器LayoutManager。适配器继承RecyclerView.Adapter类，具体实现类似ListView的适配器，取决于数据信息以及展示的UI。布局管理器用于确定RecyclerView中Item的展示方式以及决定何时复用已经不可见的Item，避免重复创建以及执行高成本的findViewById()方法。




#### 1.2 基本使用

在build.gradle文件中引入该类: 

    compile 'com.android.support:recyclerview-v7:23.4.0'
  
在需要的地方使用：

    mRecyclerView = (RecyclerView) findViewById(R.id.my_recycler_view);
    // 设置布局管理器
    mLayoutManager = new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, false);
    mRecyclerView.setLayoutManager(mLayoutManager);
    // 设置adapter
    mRecyclerView.setAdapter(mAdapter);
    // 设置Item添加和移除的动画
    mRecyclerView.setItemAnimator(new DefaultItemAnimator());
    // 设置Item之间间隔样式
    mRecyclerView.addItemDecoration(mDividerItemDecoration);

可以看见RecyclerView相比ListView会多出许多操作，ListView可能只需要去设置一个adapter就能正常使用了。RecyclerView代表的意义是，我只管Recycler View，也就是说RecyclerView只管回收与复用View，其他的你可以自己去设置。可以看出其高度的解耦，给予你充分的定制自由（所以你才可以轻松的通过这个控件实现ListView,GirdView，瀑布流等效果）。

### 2. 高级设置
#### 2.1 设置间隔样式

我们可以通过该方法添加分割线`mRecyclerView.addItemDecoration()`，该方法的参数为RecyclerView.ItemDecoration，该类为抽象类，官方目前并没有提供默认的实现类，该类是个抽象类，主要有三个方法:

* onDraw(Canvas c, RecyclerView parent, State state)，在Item绘制之前被调用，该方法主要用于绘制间隔样式
* onDrawOver(Canvas c, RecyclerView parent, State state)，在Item绘制之后被调用，该方法主要用于绘制间隔样式
* getItemOffsets(Rect outRect, View view, RecyclerView parent, State state)，设置item的偏移量，偏移的部分用于填充间隔样式，在RecyclerView的onMesure()中会调用该方法

`onDraw()`和`onDrawOver()`这两个方法都是用于绘制间隔样式，我们只需要复写其中一个方法即可。

#### 2.2 设置动画

ItemAnimator也是一个抽象类，好在系统为我们提供了一种默认的实现类，借助默认的实现，当Item添加和移除的时候，添加动画效果很简单:

    // 设置Item添加和移除的动画
    mRecyclerView.setItemAnimator(new DefaultItemAnimator());
    
在adapter中添加了两个方法：

    public void addData(int position) {
        mDatas.add(position, "Insert One");
        notifyItemInserted(position);
    }

    public void removeData(int position) {
        mDatas.remove(position);
        notifyItemRemoved(position);
    }

注意，这里更新数据集不是用`adapter.notifyDataSetChanged()`而是`notifyItemInserted(position)与notifyItemRemoved(position)` 否则没有动画效果。 

#### 2.3 设置LayoutManager


RecyclerView需要通过setLayoutManager()方法设置布局管理器，
RecyclerView提供了三种布局管理器：

* LinerLayoutManager 以垂直或者水平列表方式展示Item
* GridLayoutManager 以网格方式展示Item
* StaggeredGridLayoutManager 以瀑布流方式展示Item


实现类似GridView的功能: 

    mRecyclerView.setLayoutManager(new GridLayoutManager(this,4));
    
另外一种写法，Item的高度可以不一样
   
    mRecyclerView.setLayoutManager(new StaggeredGridLayoutManager(4,StaggeredGridLayoutManager.VERTICAL));
    
    
#### 2.4 点击事件

RecyclerView并没有像ListView一样暴露出Item点击事件或者长按事件处理的api，也就是说使用RecyclerView时候，需要我们自己来实现Item的点击和长按等事件的处理。实现方法有很多，可以监听RecyclerView的Touch事件然后判断手势做相应的处理，也可以通过在绑定ViewHolder的时候设置监听，然后通过Apater回调出去，我们选择第二种方法，更加直观和简单。

### 3. ListView 与 RecyclerView 对比
#### 3.1 缓存机制对比

ListView与RecyclerView缓存机制原理大致相似，在滑动的过程中，离屏的ItemView立即被回收至缓存，入屏的ItemView则会优先从缓存中获取，只是ListView与RecyclerView的实现细节有差异。

RecyclerView(四级缓存)比ListView(两级缓存)多两级缓存，支持多个离屏ItemView缓存，支持开发者自定义缓存处理逻辑，支持所有RecyclerView共用同一个RecyclerViewPool(缓存池)。

ListView和RecyclerView缓存机制基本一致：

* mActiveViews(ListView)和mAttachedScrap(RecyclerView)功能相似，意义在于快速重用屏幕上可见的列表项ItemView，而不需要重新createView和bindView。
* mScrapView(ListView)和mCachedViews + mReyclerViewPool(RecyclerView)功能相似，意义在于缓存离开屏幕的ItemView，目的是让即将进入屏幕的ItemView重用。
* RecyclerView的优势在于a.mCacheViews的使用，可以做到屏幕外的列表项ItemView进入屏幕内时也无须bindView快速重用；b.mRecyclerPool可以供多个RecyclerView共同使用，在特定场景下，如viewpaper+多个列表页下有优势.客观来说，**RecyclerView在特定场景下对ListView的缓存机制做了补强和完善**。


**缓存对象不一样**:

* RecyclerView缓存RecyclerView.ViewHolder，抽象可理解为：
View + ViewHolder(避免每次createView时调用findViewById) + flag(标识状态)
* ListView缓存View

RecyclerView中mCacheViews(屏幕外)获取缓存时，是通过匹配pos获取目标位置的缓存，这样做的好处是，当数据源数据不变的情况下，无须重新bindView，而同样是离屏缓存，ListView从mScrapViews根据pos获取相应的缓存，但是并没有直接使用，而是重新getView（即必定会重新bindView）

ListView中通过pos获取的是view，即pos→view；
RecyclerView中通过pos获取的是viewholder，即pos → (view，viewHolder，flag)；
从流程图中可以看出，标志flag的作用是判断view是否需要重新bindView，这也是RecyclerView实现局部刷新的一个核心。

#### 3.2 局部刷新

RecyclerView的缓存机制确实更加完善，但还不算质的变化，RecyclerView更大的亮点在于提供了局部刷新的接口，通过局部刷新，就能避免调用许多无用的bindView。

以RecyclerView中notifyItemRemoved(1)为例，最终会调用requestLayout()，使整个RecyclerView重新绘制，过程为：onMeasure()→onLayout()→onDraw()

其中，onLayout()为重点，分为三步: 

* dispathLayoutStep1()：记录RecyclerView刷新前列表项ItemView的各种信息，如Top,Left,Bottom,Right，用于动画的相关计算
* dispathLayoutStep2()：真正测量布局大小，位置，核心函数为layoutChildren()
* dispathLayoutStep3()：计算布局前后各个ItemView的状态，如Remove，Add，Move，Update等，如有必要执行相应的动画

当调用notifyItemRemoved时，会对屏幕内ItemView做预处理，修改ItemView相应的pos以及flag

当调用fill()中RecyclerView.getViewForPosition(pos)时，RecyclerView通过对pos和flag的预处理，使得bindview只调用一次

需要指出，ListView和RecyclerView最大的区别在于数据源改变时的缓存的处理逻辑，ListView是”一锅端”，将所有的mActiveViews都移入了二级缓存mScrapViews，而RecyclerView则是更加灵活地对每个View修改标志位，区分是否重新bindView

#### 3.3 总结

在一些场景下，如界面初始化，滑动等，ListView和RecyclerView都能很好地工作，两者并没有很大的差异。从性能上看，RecyclerView并没有带来显著的提升，在不需要频繁更新，暂不支持用动画的功能实现上，RecyclerView优势也不太明显，没有太大的吸引力，ListView已经能很好地满足业务需求。


列表页展示界面，需要支持动画，或者频繁更新，局部刷新，建议使用RecyclerView，更加强大完善，易扩展；其它情况(如微信卡包列表页)两者都OK，但ListView在使用上会更加方便，快捷。


  