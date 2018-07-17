---
title: Kotlin基本教程
date: 2018-06-25 17:35:58
categories: 
    - Android
tags:
    - Kotlin
---

> 初步学习的时候要学会使用Android Studio的“Convert to Kotlin”功能，当不知道kotlin怎么写的时候，可以先用java代码写，然后通过Android Studio转换为kotlin代码。

### 1. Kotlin基本语法
#### 1.1 定义变量

在Kotlin中常量用`val`声明，变量用`var`声明，关键字在前面，类型以冒号`:`隔开在后面，也可以省略直接赋值，类型后带问号`?`表示可为空类型(默认空安全)。
<!-- more -->

    // 自动推断类型
    val s = "Sample"        // 自动推断出字符串
    var i = 23              // 自动推断出整型
    i += 1                  // 变量可以改变值
    
    // 空安全变量
    var a: String = "abc"
    a = null                // 编译错误
    val l = a.length        // 可以放心地使用
    
    // 可为空字符串变量
    var b: String? = "abc"
    b = null // ok
    val l = b.length        // 错误：变量“b”可能为空
    
    // 常量数组int[][][] arrs = new int[3][2][1];
    val arrs = Array(3) { Array(2) { IntArray(1) } }

Kotlin没有隐式拓宽转换，如 Java 中`int`可以隐式转换为`long`，必须要显式类型转换。

    val a: Double = 5.2
    val b: Int = a.toInt()      // 显式转换，b 等于 5
    val c: Float = 5.2F
    val d: Int = c.toInt()      // d 等于 5
    
Kotlin中字符串模板，可以包含一些小段代码，会把求值结果合并到字符串中。
模板表达式以美元符（$）开头：

    val h = "me"
    val j = "it is $h"          // it is me
    
模板中的任意表达式，用大括号：

    val k = "h length is ${h.length}"       // h length is 2

`Array`类来创建和操作数组，和`Java`区别很大，它定义了`get`和`set`函数，`size`属性，以及一些其他有用的成员函数。
`[]`可以用于访问数组的元素，实际上`[]`被进行了操作符的重载，调用的是`Array`类的`get`和`set`方法。

    //使用装箱操作，
    val arr1 = arrayOf(1, 2, 3)
    //原生类型数组，还有 ByteArray、 ShortArray 等
    val arr2: IntArray = intArrayOf(1, 2, 3)
    //直接指定长度
    val arr3 = arrayOfNulls<Int>(5)
    //长度为 0 的空数组
    val empty = emptyArray<Int>()
    //访问数组元素
    val arr4 = arrayOf(1, 2, 3)
    println(arr4[1])         //输出“2”，建议用这个方法
    println(arr4.get(1))     //输出“2”
    //修改元素
    arr4[1] = 10
    println(arr4[1])        //输出“10”
    //遍历数组
    for (arr in arr4) {
        println(arr)
    }
    //遍历数组下标
    for (arr in arr4.indices) {
        println(arr)
    }
    
#### 1.2 条件

`if...else`正常使用。

    val l = 4
    val m = 5
    // 作为表达式
    val n = if (l > m) l else m
    
移除了`switch`用更强大的`when`替代，`when`子式可以是常量、变量、返回数值的表达式、返回`Boolean`值的表达式，强大到用来替换`if...else if`

    // 测试值 x = 0, -1, 1, 2, 3, 6, 10
    var x = 10
    when (x) {
        //常量
        2 -> println("等于2")
        //数值表达式
        if (x > 0) 1 else -1 -> println("大于0并等于1，或小于0并等于-1")
        //Boolean类型表达式
        in 1..5 -> println("范围匹配1-5")
        !in 6..9 -> println("不是6-9")
        is Int -> println("类型判断")
        else -> println("else")
    }
    // 代替if...else if
    when{
        x > 6 && x <= 10  ->  println("大于6小于等于10")
        x < 6 -> println("小于6")
        else -> println("else")
    }

#### 1.3 循环

`while` 和 `do...while` 同Java并无区别，`for`则有很大改变并多出了几个变种

    val list = arrayListOf("aa", "bb", "cc")
    //递增for (int i = 0; i < list.size(); i++)
    for (i in list.indices) {
       print(list[i])
    }
    //递增for (int i = 2; i < list.size(); i++)
    for (i in 2..list.size-1) {
       print(list[i])
    }
    //递减for (int i = list.size() - 1; i >= 0; i--)
    for (i in list.size - 1 downTo 0) {
        print(list[i])
    }
    //操作列表内的对象
    for (item in list) {
        print(item)
    }
    //加强版
    for（（i， item） in list.witnIndex()） {
        print(list[i])
        print(item)
    }
    //变种版
    list.forEach {
        print(it)
    }
    
    list.forEachIndexed { i, s ->
        print(list[i])
        print(s)
    }
    
    list.forEachIndexed(object :(Int,String) -> Unit{
        override fun invoke(i: Int, s: String) {
            print(list[i])
            print(s)
        }
    })
    
    // 根据当前的一个“book” list， 把选中的书加到一个selectedTopicsMap中
    bookList.filter { it.isSelected }
      .forEach { selectedTopicsMap.put(it.id, it) }

#### 1.4 函数声明

使用 fun 关键字声明

    //返回类型 Int
    fun sum(p: Int, q: Int): Int {
        return p + q
    }
    //表达式作为返回值
    fun sum(p: Int, q: Int) = p + q
    //函数返回无意义的值，相当于 Java 里的 void
    fun sum(p: Int, q: Int): Unit{
    }
    //Unit 返回类型可以省略：
    fun sum(p: Int, q: Int){
    }
    
#### 1.5 其它

在Kotlin中冒号`:`用万能来称呼绝不为过。常量变量的类型声明，函数的返回值，类的继承都需要它

    //val表示常量var表示变量声明
    val name: String = "tutu" 
    //省略类型说明
    var age = "23"
    //fun表示函数
    fun getName(): String{
       return "tutu"
    }
    //类继承
    class UserList<E>(): ArrayList<E>() {
        //...
    }

Kotlin最终会还是编译成Java字节码，使用到Java类是必然的，在Kotlin语法如下

    val intent = Intent(this, MainActivity::class.java)
    

用到内部类和匿名内部类以及`lambda`的时候，使用`@`确定`this`和`return`指的是哪一个

    class User {
        inner class State{
            fun getUser(): User{
                //返回User
                return this@User
            }
            fun getState(): State{
                //返回State
                return this@State
            }
        }
    }

使用kotlin实现一个Java中常用的单例

    class User {
        companion object {
            @Volatile var instance: User? = null
                get() {
                    if (field == null) {
                        synchronized(User::class.java) {
                            if (field == null)
                                field = User()
                        }
                    }
                    return field
                }
        }
    
        var name: String? = null
        var age: String? = null
    }

自定义getter/setter重点在`field`，跟我们熟悉所Java的`this`指代当前类一样，`field`指代当前参数，直接使用参数名`instance`代替不会报错但单例就没效果了。

一开始觉得lambda很高级完全看不懂，其实很简单的就是把接口名、方法名和参数类型省掉不写再加个->罢了，明白这点了很好理解。

    // 无参数无返回值
    Thread(Runnable {
        sleep(1000)
    }).start()
    // 单参数不带返回值
    view.setOnClickListener { v ->
        Log.e("tag", "${v.tag}")
    }
    // 多参数带返回值
    view.setOnKeyListener(View.OnKeyListener { v, keyCode, event ->
        Log.e("tag", "keyCode$keyCode, ${event.keyCode}")
        if (event.keyCode == KeyEvent.KEYCODE_BACK)
            return@OnKeyListener true
        false
})




内部类和参数默认为`public`，而在Java中为`default`即在同一个包中可见

类默认为不可继承(`final`)，想要可被继承要声明为`open`或`abstract`

取消了`static`关键字，静态方法和参数统一写在`companion object`块

`internal`模块内可见，`inner`内部类
    
### 2. 检查NULL

Kotlin的类型系统旨在消除来自代码空引用的危险, 在Kotlin中发生NPE的原因只有可能是以下情况:

* 显式调用`throw NullPointerException()`
* 使用了 `!!` 操作符
* 有些数据在初始化时不一致，例如当
    * 传递一个在构造函数中出现的未初始化的`this`并用于其他地方（“泄漏 this”）
    * 超类的构造函数调用一个开放成员，该成员在派生中类的实现使用了未初始化的状态；

当我们使用`?`定义变量的时候可以显式检查`b`是否为`null`

    if (b != null && b.length > 0) {
        print("String of length ${b.length}")
    } else {
        print("Empty string")
    }
这只适用于`b`是不可变的情况（即在检查和使用之间没有修改过的局部变量 ，或者不可覆盖的`val`成员），因为否则可能会发生在检查之后`b`又变为`null`的情况。

第二个选择是安全调用操作符，写作`?.`：

    b?.length

如果`b`非空，就返回`b.length`，否则返回`null`，这个表达式返回的类型是`Int?`。

安全调用在链式调用中很有用。例如，如果一个员工`Bob`可能会（或者不会）分配给一个部门， 并且可能有另外一个员工是该部门的负责人，那么获取`Bob`所在部门负责人（如果有的话）的名字，我们写作：

    bob?.department?.head?.name
    
如果任意一个属性（环节）为空，这个链式调用就会返回 null。

我们在编写代码的时候 最好把所有的`!!`都去掉，因为它会产生`NullPointerException`，

假设我们有一个kotlin类

    class RNTestActivity : AppCompatActivity() {
        var mReactInstanceManager: ReactInstanceManager? = null
        
        ...
    
        override fun onPause() {
            super.onPause()
    
            if (mReactInstanceManager != null) {
                mReactInstanceManager!!.onHostPause(this)
            }
        }
    }

其中`mReactInstanceManager `是`mutable`的，也就是说它的值是会改变的。

去掉`!!`后:

    mReactInstanceManager?.let {
             mReactInstanceManager.onHostPause(this)
    }

虽然我们做了是否为`null`的判断，但是还是报错，因为可能`mReactInstanceManager?.let`的时候值不为`null`但是运行`mReactInstanceManager(this)`的时候正好变成了`null`

再次修改: 

    override fun onPause() {
            super.onPause()
            val reactInstanceManager = mReactInstanceManager
            reactInstanceManager?.let {
             reactInstanceManager.onHostPause(this)
            }
        }
        
其中`reactInstanceManager`是一个新的variable，它是read-only，一旦赋值以后是不会变得，而且是local的。后面的`mReactInstanceManager`是上面原来的`field`变量。这样就最大程度的减少了NPE。

上面的Kotlin code还可以做的一个改进就是用 lambda,实际上这里就是做了和上面`val reactInstanceManager = mReactInstanceManager`一样的事情。如下：


    class RNTestActivity : AppCompatActivity() {
        var mReactInstanceManager: ReactInstanceManager? = null
        
        ...
    
        override fun onPause() {
            super.onPause()
    
            mReactInstanceManager?.let {
             it.onHostPause(this)
            }
        }
    }

### 3. Kotlin精简
**如果一个函数里面只有一个语句，那么可以直接用“=“简化**

Java代码:

    @Override    
    public int getItemCount() {        
        return items.size() + 1;    
    }
    
    @Override    
    public int getItemViewType(int position) {
        if (position == 0) {
            return TYPE_HEADER;        
        } else {            
            return TYPE_ITEM;        
        }    
    }
    
Kotlin代码：

    override fun getItemCount() = items.size + 1
    
    override fun getItemViewType(position: Int) = if (position == 0) TYPE_HEADER else TYPE_ITEM
    
    
**多用`when`，Mac上option + enter，“Project quick fix ”，直接可以把`if`转成`when`语句**

Java代码：

    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            if (viewType == TYPE_VIDEO_ITEM) {
                return new VideoItemViewHolder(parent);
            } else if (viewType == TYPE_IMAGE_ITEM) {
                return new ImageItemViewHolder(parent);
            } else {
                return new HeaderViewHolder(parent);
            }
            return null;
        }

Kotlin代码:

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder? {
    return when (viewType) {
            TYPE_VIDEO_ITEM -> VideoViewHolder(parent)
            TYPE_IMAGE_ITEM -> ImageItemViewHolder(parent)
            else -> HeaderViewHolder(parent)
        }
    }