---
title: React Native 基础知识
date: 2018-11-21 10:20:52
categories: 
    - React Native
tags:
    - JavaScript
---

### 1. var、let、const区别

* 使用var声明的变量，其作用域为该语句所在的函数内，且存在变量提升现象。
* 使用let声明的变量，其作用域为该语句所在的代码块内，不存在变量提升。
* 使用const声明的是常量，在后面出现的代码中不能再修改该常量的值
<!--more-->

** var 方式定义变量的一些常见问题 **

1、 作用域是函数体的全部，跳出 for 循环了，却还可以访问到 for 循环内定义的变量 a

    for(var i=0;i<10;i++){
        var a = 'a';
    }
    console.log(a);

2、 循环内变量过度共享，控制台输出了3个3

    for (var i = 0; i < 3; i++) {
        setTimeout(function () {
            console.log(i)
        }, 1000);
    }

** Let可以解决Var上面的几个问题 **

const 就是用来定义常量的，在声明的时候必须赋值，然后不能直接修改，但是可以通过以下方式修改

    const a = {a:'a'};
    a.a = 'b'


