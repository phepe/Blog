---
title: C#协程
date: 2019-04-03 16:25:02
categories: 
    - Unity3d
tags:
    - C#
---

### 1. 基本概念

协程(Coroutine)：协同程序，在主程序运行的同时，开启另外一段逻辑处理，来协同当前程序的执行。Unity的协程系统是基于C#的一个简单而强大的接口，迭代器(IEnumerator)，协程并不是多线程的，只是运行的结果很像多线程而已。他们最大的区别就是多线程可以多核并发，但是协程只能是单核按帧顺序轮转，线程可以使用多个CPU，协程不能，所以线程是真的并行。协程是在unity主线程中运行的，每一帧中处理一次，而并不与主线程并行。这就意味着在协程之间并不存在着所谓线程间的同步和互斥问题，访问同一个值也都是安全的，不会出现死锁。
<!--more-->

### 2. 协程的用法
#### 2.1 开启协程

开启协程的两种方式

* `StartCoroutine(string methodName)`。参数是方法名(字符串类型)；此方法可以包含一个参数，形参方法可以有返回值。
* `StartCoroutine（IEnumerator method)`。参数是方法名(TestMethod()),方法中可以包含多个参数；IEnumrator 类型的方法不能含有ref或者out 
类型的参数，但可以含有被传递的引用；必须有有返回值，且返回值类型为IEnumrator,返回值使用（yield retuen +表达式或者值，或者 yield break）语句。

#### 2.2 终止协程

终止协程的两种方式
* `StopCoroutine (string methodName)`，只能终止指定的协程,在程序中调用`StopCoroutine()` 方法只能终止以字符串形式启动的协程。
* `StopAllCoroutine()`，终止所有协程


#### 2.2 挂起

* `yield`：挂起，程序遇到yield关键字时会被挂起，暂停执行，等待条件满足时从当前位置继续执行

* `yield return 0` or `yield return null`:程序在下一帧中从当前位置继续执行

* `yield return 1,2,3,......`: 程序等待1，2，3...帧之后从当前位置继续执行

* `yield return new WaitForSeconds(n)`:程序等待n秒后从当前位置继续执行

* `yield new WaitForEndOfFrame()`:在所有的渲染以及GUI程序执行完成后从当前位置继续执行

* `yield new WaitForFixedUpdate()`:所有脚本中的FixedUpdate()函数都被执行后从当前位置继续执行

* `yield return WWW`:等待一个网络请求完成后从当前位置继续执行

* `yield return StartCoroutine()`:等待一个协程执行完成后从当前位置继续执行

* `yield break`:将会导致协程的执行条件不被满足，不会从当前的位置继续执行程序，而是直接从当前位置跳出函数体，回到函数的根部


### 3. 协程的执行原理


协程函数的返回值是IEnumerator，它是一个迭代器，可以把它当成执行一个序列的某个节点的指针，它提供了两个重要的接口，分别是`Current`(返回当前指向的元素)和`MoveNext`
(将指针向后移动一个单位，如果移动成功，则返回`true`)

yield关键词用来声明序列中的下一个值或者是一个无意义的值，如果使用yield `return x`(x是指一个具体的对象或者数值)的话，那么MoveNext返回为true并且`Current`被赋值为x,如果使用`yield break`使得`MoveNext`返回为`false`

如果`MoveNext`函数返回为`true`意味着协程的执行条件被满足，则能够从当前的位置继续往下执行。否则不能从当前位置继续往下执行。








 