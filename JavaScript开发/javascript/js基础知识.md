---
title: 【 JS 】基础知识
date: 2016-08-27 11:20:48
tags: JavaScript
---

## js的基本数据类型
ECMAScript 中有 5 中简单（基本）数据类型：undefined、null、Boolean、Number、String，还有一种复杂数据类型：Object。
ECMAScript 2015 新增:Symbol(创建后独一无二且不可变的数据类型)

## null，undefined 的区别？

null 	   表示一个对象是“没有值”的值，也就是值为“空”；
undefined  表示一个变量没有被声明，不存在这个值，或者被声明了但没有被赋值；

undefined不是一个有效的JSON，而null是；
undefined的类型(typeof)是undefined；
null的类型(typeof)是object；

Javascript将未赋值的变量默认值设为undefined；
Javascript从来不会将变量设为null。它是用来让程序员表明某个用 var 声明的变量时没有值的。
	
注意：
在验证null时，一定要使用　=== ，因为 == 无法分别 null 和　undefined
	null == undefined  // true
	null === undefined // false	
	
参考阅读：[undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)

## typeof 和 instanceof 操作符
typeof: "string"/"number"/"boolean"/"undefined"/"object"/"function"
例子：typeof("function(){}") === "function";
typeof(null) === "object";
typeof(true) === "boolean";
typeof(1) === "number";

## javascript 的 typeof 返回哪些数据类型

Ｏbject number function boolean underfind;


## 例举3种强制类型转换和2种隐式类型转换?

强制（parseInt,parseFloat,number）隐式（== – ===）；



## 什么是window对象? 什么是document对象?

## 执行环境和作用域的理解
执行环境定义一个变量或函数有权访问的其他数据，决定了它们的行为。每个函数都有自己的执行环境，当执行流进入一个函数时，函数的环境就会被推入一个环境栈中，在函数执行之后，栈将环境弹出，并把控制权返回给之前的执行环境。当代码在一个环境中执行时，会创建变量对象的一个**作用域链**，作用域链的用途是**保证对执行环境有权访问的所有变量和函数的有序访问**。

1.作用域链的前端，始终是当前执行的代码所在环境的**变量对象**。如果这个环境是函数，则**将其活动对象作为变量对象**。活动对象在最开始只包含一个变量，即 arguments 对象。作用域链中的下一个变量对象来自包含环境，而下一个变量对象来自下一个包含环境。这样一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链的最后一个对象。
2.标识符解析是沿着作用域一级一级地搜索标识符的过程。搜索过程从作用域链的前端开始，逐级向后回溯，直至找到标识符为止。内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。

### 延长作用域链
- try-catch 语句的 catch 块
- with 语句

### 没有块级作用域
```
for(var i = 0; i < 10; i++){
	doSomething(i);
}
alert(i);    // 10

/* for 循环结束后，依旧会存在于循环外部的执行环境中。 */
```

## 垃圾收集机制
JS 有自动垃圾收集机制，执行环境会负责管理代码执行过程中使用的内存。原理是找出那些不再使用的变量，然后释放其占用的内存。

### 标记清除
方式：变量进入环境时，将变量标记为“进入环境”；变量离开环境时，将其标记为“离开环境”。垃圾收集器在固定时间间隔完成内存清除工作，销毁离开环境的变量并回收所占用的内存空间。

### 引用计数
记录每个值被应用的次数，当引用次数变为0时，清除这个值。但是这种方法**存在严重的问题：循环引用。**

### 那些操作会造成内存泄漏？

内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。

1.setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
2.闭包（不加节制地使用很多闭包）、控制台日志
3.循环引用（采用引用计数的垃圾回收机制）
```
// 循环引用的例子
function problem(){
	var objA = new Object();
	var objB = new Object();

	objA.someOtherObj = objB;
	objB.anotherObj = objA;
}
```
objA 和 ObjB 通过各自的属性相互引用，两个对象的引用次数都是2，永远不会是0，因此永远不会被垃圾回收机制回收。如果这个函数被多次引用，将会导致内存泄漏。
4.IE 中有一部分对象不是原生 JS 对象，例如其 BOM 和 DOM 中的对象就是使用 C++ 以 COM(组件对象模型)对象的形式来实现的，而 COM 对象采用的是引用计数策略进行垃圾回收。因此，即使 IE 的 JS 引擎是使用标记清除策略来实现的，只要在 IE 中涉及 COM 对象，就会存在循环引用的问题。
```
// IE 中使用 COM 对象导致的循环引用问题
var element = document.getElementById("some_element");
var myObject = new Object();

myObject.element = element;
element.someObj = myObject;
```
上面的例子在一个 DOM 元素 element（内部使用COM形式实现）与一个原生 JS 对象（myObject）之间创建了循环引用。由于存在这个循环引用，即使将例子中的 DOM 从页面中移除，它也永远不会被回收。
解决的方法是手工断开连接：`myObject.element = null`，`element.someObj = null;`。

注意：**IE9**把BOM 和 DOM 对象都转换成了真正的 JS 对象。这样就避免了两种垃圾收集算法并存导致的问题。























