---
layout: post
title: JavaScript函数调用的各种方法
date: 2017-7-16
categories: blog
tags: [JavaScript,函数,调用,函数调用]
description: JavaScript
---

**在JavaScript中一共有下面4种调用方式：**

* [基本函数调用](#基本函数调用)  
* [方法调用](#方法调用)  
* [构造器调用](#构造器调用)  
* [通过call()和apply()进行调用](#通过call和apply进行调用)
#### 基本函数调用  
普通函数调用模式，如：

    JavaScript code
    
    function fn(o){
    …… 
    }
    fn({x:1});
在基本函数调用中，
* 1.每个参数作为实参传递给声明函数时定义的形参；  
* 2.this被绑定到全局变量（直接调用一般指的是window）

>[code=javascript]


>  var object = {value:1};

> var value = 2;
> object.printProps = function(){

>    var printValue = function(){

>      console.log(this.value);

>    };

>   printValue();

>   console.log(this.value);

> }

> object.printProps();

>[/code]


此时的运行结果是：  
2   
1

在printValue()函数在执行时，this.value值为2，也就是说，this指向的是全局对象，而不是对象object。

* 返回值：函数的返回值成为调用表达式的值。
    I. 如果函数返回是解释器到达结尾，也就是没有执行到return XXX的语句。返回值为undefined。 
    II. 如果函数返回是因为接受器执行到return xxx语句， 返回return之后的值。 
    III. 如果return语句后没有值，即return，则返回undefined。

#### 方法调用

当一个函数被保存为对象的一个属性时，称为方法。
（1）参数和返回值的处理与函数调用一致；
（2）调用上下文this为该对象
>[code=javascript] 
>function print(){
>    console.log(this.value); 
>  }
>  var value=1;>  var object = {value:2};
>  object.m = print;
>  //作为函数调用
>  print();
>  //作为方法调用
>  object.m();
>[/code]
运行结果为：
1
 2

当调用print时，this绑定的是全局对象，打印全局变量value值1。
但是当调用object.m()时，this绑定的是方法m所属的对象Object，所以打印的值为Object.value，即2。

#### 构造器调用

 函数或方法调用之前带有关键字new，它就构成构造函数调用。如：
>[code=javascript]
> function fn(){……}
> var obj = new fn();
>[/code]
（1）参数处理：一般情况构造器参数处理和函数调用模式一致。但如果构造函数没用形参，JavaScript构造函数调用语法是允许省略实参列表和圆括号的。

如：下面两行代码是等价的。
>[code=javascript]
>  var obj = new Object();
>  var obj = new Object;
>[/code]
（2）函数的调用上下文为新创建的对象。
>[code=javascript]
> function fn(value){
>   this.value =value;
> }
> var value =1;
> var obj = new fn(2);
> console.log(value);
> console.log(obj.value);
>fn(3);
>console.log(value);
>console.log(obj.value);
>[/code]

运行结果：  
 1  
 2  
 3  
 2

 I. 第一次调用fn()函数是作为构造函数调用的，此时调用上下文this被绑定到新创建的对象obj。所以全局变量value值不变，obj新增一个属性value，值为2；
 II. 第二次调用fn()函数是作为普通函数调用的，此时调用上下为this被绑定到全局对象，在浏览器中为window。所以全局对象的value值改变为3，而obj的属性值不变。

（3）构造函数通常不使用return关键字，返回值为新对象。而如果构造函数显示地使用return语句返回一个对象，那么调用表达式值就为这个对象。如果构造函数使用return语句但没有指定返回值或者返回一个原始值，则忽略返回值，同时使用新对象作为调用结果。

#### 通过call和apply进行调用

虽然在一个独立的函数调用中，根据是否是strict模式，this指向undefined或window，不过，我们还是可以控制this的指向的！要指定函数的this指向哪个对象，可以用函数本身的apply()或call()方法，它们都是接收两个参数。

* call()方法使用它自有的实参列表作为函数的实参，apply()方法要求以数组的形式传入参数

* apply方法第一个参数是改变后的调用对象，第二个参数是将函数参数以数组形式传入[ ]，而call()第一个参数与call()方法相同，第二个参数是原来参数序列......。
