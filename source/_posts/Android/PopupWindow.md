---
title: PopupWindow使用详解
date: 2018-07-05 11:35:28
categories: 
    - Android
tags:
    - Android
---

> PopupWindow是一个以弹窗方式呈现的控件，可以用来显示任意视图，它会浮动在当前活动的activity顶部，因此我们可以通过PopupWindow实现各种各样的弹窗效果，PopupWindow自定义布局比较方便，而且在显示位置比较自由不受限制（与AlertDialog的主要区别是AlertDialog不方便指定显示位置，默认显示在屏幕最中间）。


### 1. PopupWindow的相关函数
#### 1.1 构造函数


    //方法一：
    public PopupWindow (Context context)
    //方法二：
    public PopupWindow(View contentView)
    //方法三：
    public PopupWindow(View contentView, int width, int height)
    //方法四：
    public PopupWindow(View contentView, int width, int height, boolean focusable)


<!-- more -->

要生成一个PopupWindow最基本的三个条件是：`contentView、width、height`少任意一个就不可能弹出来PopupWindow

`contentView`为要显示的视图，`width`和`height`为宽和高，值为像素值，也可以是`MATCHT_PARENT`和`WRAP_CONTENT`。`focusable`设置是否获取焦点，默认为`False`。

PopupWindow是没有默认布局的，所以必须设置`contentView`，`width`和`height`不设置默认为0，就显示不出来PopupWindow。


#### 1.2 显示函数

显示函数主要使用下面三个：

    //相对某个控件的位置（正左下方），无偏移
    showAsDropDown(View anchor)：
    //相对某个控件的位置，有偏移;xoff表示x轴的偏移，正值表示向左，负值表示向右；yoff表示相对y轴的偏移，正值是向下，负值是向上；
    showAsDropDown(View anchor, int xoff, int yoff)：
    //相对于父控件的位置（例如正中央Gravity.CENTER，下方Gravity.BOTTOM等），可以设置偏移或无偏移
    showAtLocation(View parent, int gravity, int x, int y)：
    
    
### 2. 高级功能
#### 2.1 添加阴影

使PopupWindow的界面充满全屏，而实际的列表菜单只是其中的一个子布局即可，在可见内容外面包一层`RelativeLayout`,给`RelativeLayout`添加了半透明背景`android:background="#66000000"`
   
#### 2.2 添加动画

为PopupWindow添加动画并不难，只需要使用一个函数即可，可以设置显示和退出动画 ：

    //设置动画所对应的style
    mPopWindow.setAnimationStyle(R.style.contextMenuAnim);

#### 2.3 常用函数讲解

`setTouchable(boolean touchable)`: 设置PopupWindow是否响应touch事件，默认是true，如果设置为false，所有touch事件无响应，包括点击事件。

`setFocusable(boolean focusable)`: 设置PopupWindow是否具有获取焦点的能力，默认为False。一般来讲是没有用的，因为普通的控件是不需要获取焦点的，而对于EditText则不同，如果不能获取焦点，那么EditText将是无法编辑的。

`setOutsideTouchable(boolean touchable)`: 设置PopupWindow以外的区域是否可点击，即如果点击PopupWindow以外的区域，PopupWindow是否会消失。

`setBackgroundDrawable(Drawable background)`: 这个函数不仅仅是设置背景，只有加上它之后，`setOutsideTouchable（）`才会生效、PopupWindow才会对手机的返回按钮有响应，如果不加`setBackgroundDrawable（）`点击手机返回按钮将关闭的PopupWindow所在的Activity。一般我们设置一个空的bimmap就行`setBackgroundDrawable(new BitmapDrawable())`

如果我们给PopupWindow设置了`setBackgroundDrawable`，系统会在我们设置的contentView外再包一层布局，那它将会产生的作用是：

* setOutsideTouchable(true)将生效，具有外部点击隐藏窗体的功能
* 手机上的返回键将可以使窗体消失
* 对于PopupWindow上层没有捕捉的点击事件，点击之后，仍然能使窗体消失

### 3. 其他
#### 3.1 为什么要强制代码设置PopupWindow的Height、Width

设置contentView很容易理解，但width和height为什么要强制设置呢？我们在布局代码中不是已经写的很清楚了么？

因为控件的大小，是建立在父控件大小确定的基础上的，而我们创建`contentView`的时候

    View contentView = LayoutInflater.from(MainActivity.this).inflate(R.layout.popuplayout, null);
    
直接inflate出来的，我们对它没有设置根结点！
那问题来了？它的大小由谁来解决呢？
好像没有谁能决定了，因为他没有父结点。那它到底是多大呢？未知！
所以只有通过代码让用户去手动设置了！所以这就是为什么非要用户设置width和height的原因了。

`PopupWindow`窗体的宽度和高度都是通过mWidth和mHeight来设置的，而mWidth和mHeight只有通过构造函数设置，如果我们没有设置mWidth和mHeight，那mWidth和mHeight将会取默认值0，所以当我们没有设置width和height时，并不是我们的窗体没有弹出来，而是因为他们的width和height都是0了。
