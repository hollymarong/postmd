---
title: 闭包
date: 2017-04-11 07:50:36
tags: JavaScript
---
* 堆
堆是一种无序的数据结构，数据的存取方式根据引用直接获取，现实生活中常见的堆的例子：书架与书。
* 栈
栈是一种高效的数据结构，只能在栈顶添加或删除数据，是一种后入先出(LIFO, last-in-first-out)的数据结构。现实生活常见的栈的例子：摞盘子。
JavaScript没有严格意义上区分堆内存和栈内存。在JavaScript的执行环境在逻辑上实现了堆栈。
执行环境中的变量对象保存在堆内存中。
基础数据类型存储在栈内存中，基础数据类型是按值访问的，可以直接操作保存在变量终端额实际值。
引用数据类型的值是保存在堆内存中，引用类型的值是按引用访问的，不允许直接访问对内存中的位置，操作对象时实际是操作的对象的引用。

| 比较         | 栈内存             | 堆内存                       |
| :---         | :---               | :---                         |
|              |                    |                              |
| 存储数据类型 | 基础数据类型       | 引用数据类型                 |
| 存储值       | 大小固定           | 大小不固定，可动态调整       |
| 内存分配     | 编译器自动分配释放 | 程序分配                     |
| 用途         | 主要用来执行程序   | 主要用来存放对象             |
| 特点         | 空间小，运行效率高 | 空间大，运行效率相对较低     |
| 存取方式     | 先进后出，后进先出 | 无序存储，可根据引用直接获取 |
* 调用栈

```
function printCallStack() {
    var i = 0;
    var fun = arguments.callee;
    do {
        fun = fun.arguments.callee.caller;
        console.log(++i + ': ' + fun);
    } while (fun);
}

function c() {
    console.log('c');
    printCallStack();
}

function b() {
    console.log('b');
    c();
}

function a() {
    console.log('a');
    b();
}

a();
```

* 执行环境(execution context)
JavaScript执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。
每个执行环境都有一个与之关联的变量对象(variable object),环境中定义的所有变量和函数都保存在这个对象中，我们编写的代码无法访问这个对象，但解析器在处理数据的时候会再后台使用它。
某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁.全局执行环境是最外围的一个执行环境（全局执行环境直到应用程序退出才会被销毁）。
JavaScript程序的执行流的机制: 每个函数都有自己的执行环境，当执行流进入一个函数时，函数的环境就会被推入到一个环境栈中，当函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。

* 作用域链(scope chain)：
当代码在一个环境中执行时，会创建变量对象的一个作用域链。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。
作用域链的前端，始终是当前执行的代码所在环境的变量对象,作用域链中的下一个变量来自包含环境，一直延续到全局执行环境，全局执行环境的变量对象始终是作用域链中的最后一个对象。
标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级向后，直到找到标识符为止(如果找不到标识符，通常会导致错误发生).

延长作用域链的两种情况：
* with语句
* try/catch语句中catch语句

```javascript
function buildUrl(){
    var qs = "?debug=true";
    with(location){
        var url = href + qs;
    }
    return url;
}
```
with语句接收location对象，with语句的变量对象里包含了location对象,with语句引用href时，引用的是location.href

```javascript
function lengthenScopeChain(){
    try{
        var dd = 2;
        throw 'create error';
    } catch(e) {
        console.log('dd inner catch',dd);  // 2
        console.log('e', e); // create error
    }
    console.log('dd outer',dd); // 2
    console.log('e outer', e);  //报错，e is not defined
}
lengthenScopeChain();
```
catch语句接收e对象，catch语句的变量对象里包含了e对象

* 作用域
1. 块级作用域
在一些类似C语言的编程语言中，花括号内的每一段代码都具有各自的作用域，而且变量在声明它们的代码段之外是不可见的，我们称为块级作用域。

2. javascript函数作用域
JavaScript中没有块级作用域，Javascript使用了函数作用域，就是函数的执行环境：变量在声明它们的函数体内以及这个函数体嵌套的任意函数体内都是有定义的。
Javascript的函数作用域是指在函数内声明的所有变量在函数体内始终是可见的,这意味着变量在声明之前甚至已经可用，Javascript的这个特性被非正式地称为申明提前,即Javascript函数里声明的所有变量(但不涉及赋值)都被提前至函数体的顶部,函数声明提前这步操作是在Javscript引擎预编译时进行的，是在代码开始运行之前。
在函数体内，局部变量的优先级高于同名的全局变量。
如果在函数声明的一个局部变量或者函数参数中带有的变量和全局变量重名，局部变量会覆盖全局变量

```javascript
//函数内的变量会覆盖同名的全局变量
var name = 'global';
function checkScope(){
    console.log(name);
    var name = 'local';
}
checkScope() //undefined

//函数内的参数会覆盖同名的全局变量
var name = 'global';
function checkScope(name){
    console.log(name);
}
checkScope(); //undefined

```

* 作用域链

闭包就是内部函数可以访问定义他们的外部函数的变量和参数(除了this，arguments),也就是函数可以访问存在于该函数声明时的作用域内的变量和函数。
特点：
1. 闭包可以访问函数声明时所在作用域内的变量和函数
2. 即使外部函数已经返回，当前函数仍然可以引用外部函数所定义的变量
3. 闭包可以更新外部变量的值，闭包存储的是外部变量的引用.
```javascript
function scope(){
    var value;
    return {
        get: function(){
            return value;
        },
        set: function(newValue) {
            value = newValue;
        }
    }
}
var obj = scope();
obj.get(); //undefined
obj.set('marong');
obj.get(); //marong

```
优点：
1.避免全局变量的污染
2.减少代码的数量和复杂性
缺点：
1.会比其他函数占内存。
由于闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存。
 
* 内存
* 内存的声明周期
1. 分配你所需要的内存
2. 使用分配到的内存（读，写）
3. 不需要是将其释放\归还

* Javascript垃圾回收机制
JavaScript具有自动垃圾回收机制，执行环境会负责管理代码执行过程中使用的内存,开发人员不用关心内存的使用问题，所需内存的分配以及无用内存的回收完全实现了自动管理。这种垃圾收集机制的原理：找到那些不再继续使用的变量，然后释放其占用的内存。垃圾回收器会按照固定的时间间隔，周期性的执行这一操作。
局部变量的生命周期，局部变量只在函数执行的过程中存在，在这个过程中，会为局部变量在堆或栈内存上分配相应空间，以便存储它们的值，然后在函数中使用这些变量，函数运行结束后，判断局部变量是否有用,无用变量进行标识，以备将来回收。
浏览器中标识无用变量的策略，通常有两种。
1. 标记清除
JavaScript中最常用的垃圾收集方式是标记清除(mark-and-sweep)。当变量进入环境是，将这个变量标记为“进入环境”,当变量离开环境时，将其标记为“离开环境”。垃圾收集器在运行的时候会给内存种所有的变量都加上标记，然后它会去掉环境中的变量，根据标记删除无用变量，回收它们占用的内存空间。
IE， Firefox,Opera,Chrome Safari的Javascript实现使用的是标记清除式的垃圾收集策略，垃圾收集的时间间隔不同。
2. 引用计数
引用计数的含义是跟踪记录每个值被引用的次用。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1，如果同一个值又被赋给另一个变量，则该值的引用次数加1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减1.当这个值的引用次数变成0时。当垃圾收集器运行时，释放引用次数为0的值所占用的内存。
引用计数的策略在循环引用的时候，会出现问题。
```
function problem(){
    var objA = new Object();
    var objB  = new Object();
    objA.someOther = objB;
    objB.someOther = objA;
}

```
当函数执行完成之后，objA,objB的引用次数都为2，垃圾回收器无法回收，如果这个函数被多次调用，就会导致大量内存得不到回收。为此，Netscape在Navigator4.0中放弃了引用计数的策略，使用标记清除的策略。
IE中有些对象不是原生的Javascript，其中BOM/DOM对象是使用C++以COM(Component Object Model, 组件对象模型)形式实现的。COM对象的垃圾收集策略采用的引用计数的方式，只要在IE中设计到COM，就会存在循环引用的问题。

```
var element = document.getElementById('some');
var obj = new Object();
obj.element = element;
element.someobj = obj;
```

即使DOM中element元素被移除，element也不会被垃圾回收器回收。
为了避免类似这种循环引用的问题，最好在不使用它们的时候，手动断开连接
将变量值设为null,切断变量和它之前引用值的连接，当垃圾收集器下次运行时，就会删除这些值并回收它们的内存。
IE9中BOM/DOM对象都是原声的Javascript对象，避免了引用计数循环引用的问题，消除了常见内存泄漏的现象。

```
obj.element = null;
element.someobj = null;
```

* 内存管理
解除引用(dereferencing)：将其中设置为null来释放其引用，解除一个值的引用的作用是让值脱离执行环境，以便垃圾收集器下次运行时将其回收
分配给web浏览器的内存通常比一般的桌面应用程序少，出于安全的考虑，防止浏览器耗尽系统内存而崩溃。确保占用较少的内存，能保证页面的性能。
优化内存占用的最佳方式，执行的代码中只保存必要的数据。一旦数据不再有用，可以解除引用。
```
function createPerson(){
    return {
        name:'person'
    }
}
var globalPerson = createPerson;
//dereferencing
globalPerson = null;

```
* 内存溢出
程序申请内存时，没有足够的内存空间供其使用。
* 内存泄漏
是指程序申请内存后，又不能回收,无法释放已申请的内存空间,内存泄漏会降低页面性能，还可能会导致程序出错或浏览器崩溃。内存泄漏可能会导致内存溢出。

