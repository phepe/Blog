---
title: React Native 基础知识
date: 2018-06-21 10:20:52
categories: 
    - React Native
tags:
    - React Native
---

### 1. 前导

* ES2015 （也叫作 ES6）: 是一套对`JavaScript`的语法改进的官方标准。但是这套标准目前还没有在所有的浏览器上完整实现，所以目前而言 web 开发中还很少使用。React Native 内置了对 ES2015 标准的支持，你可以放心使用而无需担心兼容性问题。`import`、`from`、`class`、`extends`、以及`() =>`箭头函数等新语法都是 ES2015 中的特性。

<!--more-->
* JSX: 是一种在`JavaScript`中嵌入`XML`结构的语法。`<View><Text>Hello world!</Text></View>`就是标准的JSX语法，它是让你在代码中嵌入结构标记，上面的示例代码中，使用的是内置的`<Text>`组件，它专门用来显示文本，而`<View>`就类似`html`中的`div`或是`span`这样的容器。我们可以把任意合法的 JavaScript 表达式通过括号嵌入到 JSX 语句中

* 组件（Component）: 编写 React Native 应用时，肯定会写出很多新的组件。而一个 App 的最终界面，其实也就是各式各样的组件的组合。组件本身结构可以非常简单，唯一必须的就是在`render`方法中返回一些用于渲染结构的 JSX 语句。


### 1. Props属性

大多数组件在创建时就可以使用各种参数来进行定制。用于定制的这些参数就称为`props`（属性）,以常见的基础组件Image为例，在创建一个图片时，可以传入一个名为`source`的`prop` 来指定要显示的图片的地址，以及使用名为`style`的`prop`来控制其尺寸。

### 2. State（状态）

我们使用两种数据来控制一个组件：`props`和`state`。`props`是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变。 对于需要改变的数据，我们需要使用`state`。一般来说，你需要在`constructor`中初始化`state`，然后在需要修改时调用`setState`方法。

* 一切界面变化都是状态`state`变化
* `state`的修改必须通过`setState()`方法
    * this.state.likes = 100; // 这样的直接赋值修改无效！
    * setState 是一个 merge 合并操作，只修改指定属性，不影响其他属性
    * setState 是异步操作，修改不会马上生效

### 3. Style（样式）

所有的核心组件都接受名为`style`的属性。这些样式名基本上是遵循了`web`上的`CSS`的命名，只是按照`JS`的语法要求使用了驼峰命名法，例如将`background-color`改为`backgroundColor`。
`style`属性可以是一个普通的`JavaScript`对象。这是最简单的用法，因而在示例代码中很常见。你还可以传入一个数组——在数组中位置居后的样式对象比居前的优先级更高，这样你可以间接实现样式的继承。

实际开发中组件的样式会越来越复杂，我们建议使用`StyleSheet.create`来集中定义组件的样式。

最简单的给组件设定尺寸的方式就是在样式中指定固定的width和height。React Native 中的尺寸都是无单位的，表示的是与设备像素密度无关的逻辑像素点。

在组件样式中使用`flex`可以使其在可利用的空间中动态地扩张或收缩。一般而言我们会使用`flex:1`来指定某个组件扩张以撑满所有剩余的空间。如果有多个并列的子组件使用了`flex:1`，则这些子组件会平分父容器中剩余的空间。如果这些并列的子组件的`flex`值不一样，则谁的值更大，谁占据剩余空间的比例就更大，即占据剩余空间的比等于并列组件间`flex`值的比。组件能够撑满剩余空间的前提是其父容器的尺寸不为零。如果父容器既没有固定的`width`和`height`，也没有设定`flex`，则父容器的尺寸为零。其子组件如果使用了`flex`，也是无法显示的。


### 4. Flexbox布局

在RN中使用flexbox规则来指定组件子元素的布局，Flexbox可以在不同屏幕尺寸上提供一致的布局结构。一般来说，使用`flexDirection`、`alignItems`和 `justifyContent`三个样式属性就已经能满足大多数布局需求。

#### 4.1 Flex Direction

在组件的`style`中指定`flexDirection`可以决定布局的主轴。子元素是应该沿着水平轴(row)方向排列，还是沿着竖直轴(column)方向排列，默认值是竖直轴(column)方向。

#### 4.2 Justify Content

在组件的`style`中指定`justifyContent`可以决定其子元素沿着主轴的排列方式。子元素是应该靠近主轴的起始端还是末尾段分布，亦或应该均匀分布，对应的这些可选项有：`flex-start`、`center`、`flex-end`、`space-around`、`space-between`以及`space-evenly`。

#### 4.3 Align Items
在组件的`style`中指定`alignItems`可以决定其子元素沿着次轴（与主轴垂直的轴，比如若主轴方向为`row`，则次轴方向为`column`）的排列方式。子元素是应该靠近次轴的起始端还是末尾段分布呢，亦或应该均匀分布，对应的这些可选项有：`flex-start`、`center`、`flex-end`以及`stretch`。要使`stretch`选项生效的话，子元素在次轴方向上不能有固定的尺寸。
