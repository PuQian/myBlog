---
title: 【 JS 】编程实践
date: 2017-02-07 11:57:48
tags: JavaScript
---
# UI 层的松耦合
## 什么是松耦合
本质上讲，每个组件需要保持足够瘦身来确保松耦合。

## 将 JavaScript 从 CSS 中抽离
CSS 表达式包裹在一个特殊的 expression() 函数中，可以给其传入 JavaScript 代码。
```
// 不推荐的写法
.box{
	width: expression(document.body.offsetWidth + "px");
}
```
IE9 已经不再支持 CSS 表达式了，但是老版本的 IE 依然可以运行 CSS 表达式，**推荐不要使用 CSS 表达式**。

## 将 CSS 从 JavaScript 中抽离
当需要通过 JavaScript 来修改元素样式的时候，最佳方法是操作 CSS 的 className。JavaScript 不应当直接操作样式，以便保持和 CSS 的松耦合。
```
// 不推荐的写法
element.style.cssText = "color: #fff; margin-left: 10px;";

// 推荐的写法
element.className += "active";
```
注意：可以使用 style.top、style.left、style.bottom、style.right 来对元素做定位，在 css 中定义这个元素的默认属性，而在 js 中修改这些默认值。

## 将 JavaScript 从 HTML 中抽离
**推荐使用 js 方法来添加事件处理程序**。
```
// 不推荐的写法
<button onclick="doSomething();" id="action-btn">Click Me</button>

// 推荐的写法
function addListener(target, type, handler) {
	if (target.addEventListener) {
		target.addEventListener(type, handler, false);
	} else if (target.attachEvent) {
		target.attachEvent("on" + type, handler);
	} else {
		target["on" + type] = handler;
	}
}

function doSomething() {
	// 代码
}

var btn = document.getElementById("action-btn");
addListener(btn, "click", doSomething);
```

## 将 HTML 从 JavaScript 中抽离
```
// 不推荐的写法
var div = document.getElementById("my-div");
div.innerHTML = "<h3>Error</h3><p>Invalid email address.</p>";
```
**解决办法~~~** 
- 从服务器加载
- 简单客户端模板
- 复杂客户端模板

<br>

# 避免使用全局变量
## 全局变量带来的问题
创建全局变量被认为是糟糕的实践，可能带来很多问题：**命名冲突、代码的脆弱性、难以测试**。当定义函数时，最好尽可能多地将数据置于局部作用域内，任何来自函数外部的数据都应当以参数形式传进来。这样做可以将函数和其外部环境隔离开来，并且你的修改不会对程序的其他部分造成影响。任何依赖全局变量才能正常工作的函数，只有为其重新创建完整的全局环境才能正确地测试它.

## 意外的全局变量
```
function doSomething() {
	var count = 10;
		name = "matuan"; // 创建了全局变量
}
```
这段代码出现了严重的错误，不仅创建了全局变量，而且 name 实际上是 window 的一个默认属性。推荐总是使用 var 来定义变量，哪怕是定义全局变量。

## 单全局变量方式
**推荐依赖尽可能少的全部变量，只创建一个全局变量**。
```
// 推荐的写法
var Custem = {};

Custem.Book = function(title) {
	this.title = title;
	this.page = 1;
}

Custem.Book.prototype.turnPage = function(direction) {
	this.page += direction;
}

Custem.Chapter1 = new Custem.Book("js");
Custem.Chapter2 = new Custem.Book("css");
```

### 命名空间
推荐每个文件创建新的全局对象来声明自己的命名空间。

### 模块
**AMD（异步模块定义）规范~~~**，类似的还有 **CommonJS 规范**，CommonJS 规范更适用于服务器端或纯文本编程，在浏览器端不太适用。

## 零全局变量
使用一个立即执行的函数调用并将所有脚本放置其中。
```
(function(win){
	var doc = win.document;

	// 其他代码
}(window));
```
这段脚本可以注入到页面中而不会产生任何全局变量。

<br>

# 事件处理
## 典型用法
事件对象（event 对象）会作为回调参数传入事件处理程序中。

## 规则一：隔离应用逻辑
**推荐将应用程序从所有事件处理程序中抽离出来**，有益于测试。

## 规则二：不要分发事件对象
最好让事件处理程序成为接触到 event 对象的唯一的函数。事件处理程序应当在进入应用逻辑之前针对 event 对象执行任何必要的操作，包括阻止默认事件或阻止事件冒泡。

<br>

# 避免 “空比较”
## 检测原始值
在 js 中，有 5 种原始类型：String、Number、Boolean、null 和 undefined。

**检测原始值的方法**
- 对于字符串，typeof 返回 "string"；
- 对于数字，typeof 返回 "number"；
- 对于布尔值，typeof 返回 "boolean"；
- 对于 undefined，typeof 返回 "undefined"；

- 对于 null，typeof 返回 "object"；

## 检测引用值
内置引用类型：Object、Array、Date 和 Error。typeof 运算符判断这些引用类型都返回 "object"。

**检测引用值的方法是使用  instanceof 运算符**，instanceof 运算符不仅检测构造这个对象的构造器，还检测原型链。需要注意：假设两个浏览器帧（frame）拥有一个对象的同一份拷贝，即使定义是完全一样的，instanceof 运算符仍然不能检测为是同一个对象。

### 检测函数
检测函数最好的方法使用 typeof，因为它可以跨帧使用。

### 检测数组
```
function isArray(value) {
	if(typeof Array.isArray === "function") {
		return Array.isArray(value);
	} else {
		return Object.prototype.toString.call(value) === "[object Array]";
	}
}
```

## 检测属性
**检测属性最好的方法是使用 in 运算符**，如果只想检查实例对象的某个属性是否存在，则使用 hasOwnProperty() 方法。

<br>

# 将配置数据从代码中分离出来
## 什么是配置数据
- URL
- 需要展现给用户的字符串
- 重复的值
- 设置（比如每页的配置项）
- 任何可能发生变更的值
原则是，配置数据是可以发生变更的，并且你不希望因为有人突然想改修页面展示的信息，而导致你去修改 js 源码。

## 抽离配置数据
可以将整个 config 对象放到单独的文件中，这样对配置数据的修改可以完全和使用这些数据的代码隔离开来。

## 保存配置数据
**推荐采用 Java 属性文件（Java properties file）来保存配置数据）

<br>

# 抛出自定义错误
## 在 JavaScript 中抛出错误
Error 对象时最常用的错误对象，它的构造器只接受一个参数，代指错误消息（message）。
```
throw new Error("Something bad happened.");
```

## 抛出错误的好处
抛出自己的错误可以使用确切的文本供浏览器显示，除了行和列的号码，还可以包含任何你需要的有助于调试问题的信息。**推荐总是在错误消息中包含函数名称，以及函数失败的原因。
```
// 推荐的写法
function getDivs (element) {
	if(element && element.getElementByTagName) {
		return element.getElementByTagName("div");
	} else {
		throw new Error("getDivs(): Argument must be a DOM element.");
	}
}
```

## 何时抛出错误
**推荐应该抛出错误的地方：**
- 一旦修复了一个很难调试的错误，尝试增加一两个自定义错误；
- 如果正在编写代码，思考一下：“我希望[某些事情]不会发生，如果发生，我的代码会一团糟”。这时，如果“某些事情”发生，就抛出一个错误；
- 如果正在编写的代码别人也会使用，思考一下他们的使用方式，并在特定的情况下抛出错误；

## try-catch 语句
可能引发错误的代码放在 try 块中，处理错误的代码放在 catch 块中。当然，还可以增加一个 finally 块，finally 块中代码不管是否有错误发生，最后都会被执行。如果 try 块中包含了一个 return 语句，实际上必须等到 finally 块中的代码执行后才能返回，因此 finally 块并不是很常用。

## 错误类型
ECMA-262 规范指出了 7 种错误类型：
- `Error` 所有错误的基本类型，实际上引擎从来不会抛出该类型的错误；
- `EvalError` 通过 eval() 函数执行代码发生错误时抛出；
- `RangeError` 超出边界时抛出；
- `ReferenceError` 期望的对象不存在时抛出； 
- `SyntaxError` 给 eval() 函数传递的代码中有错误时抛出（语法错误）；
- `TypeError` 变量不是期望的类型时抛出；
- `URIError` 传递非法的 URI 字符串时抛出；

**创建自定义错误类型实例：~~~**
```
// 创建自定义错误类型
function MyError(message) {
	this.message = message;
}
MyError.prototype = new Error();

// 1. 用 throw 抛出一个 MyError 的实例对象
throw new MyError("Something bad happended");

// 2. 用 try-catch 块抛出错误
try {
	// 有些代码引发了错误
} catch (ex) {
	if (ex instanceof MyError){
		// 处理自己的错误
	} else {
		// 其他处理
	}
}
```

<br>

# 不是你的对象不要动
## 什么是你的
当你的代码创建了这些对象时，你拥有这些对象；同样，如果你的代码没有创建这些对象，那么不要修改它们。
**不能修改的对象包括：**
- 原生对象（Object、Array 等等）；
- DOM 对象（例如：document）；
- 浏览器对象模型（BOM）对象（例如：window）；
- 类库的对象

## 原则
不覆盖方法、不新增方法、不删除方法

## 更好的途径
**推荐不直接修改对象而是扩展对象**，有两种基本的形式：基于对象的继承和基于类型的继承。

### 基于对象的继承
```
var person = {
	name : 'Alice',
	sayName : function() {
		alert(this.name);
	}
};
var myPerson = Object.create(person);
myPerson.sayName();   // 弹出 "Alice"

myPerson.sayName = function() {
	alert("Bob");
}
myPerson.sayName();  // 弹出 "Bob"
person.sayName();    // 弹出"Alice"
```

### 基于类型的继承
基于类型的继承是通过构造函数实现的，而不是对象。
```
// 原型继承
function MyError(message) {
	this.message = message;
}
MyError.prototype = new Error();

var error = new MyError("Something bad happended.");

console.log(error instanceof Error);  // true
console.log(error instanceof MyError);  // true

// 继承构造器
function Person(name) {
	this.name;
}

function Author(name) {
	Person.call(this, name);  // 继承构造器
}

Author.prototype = new Person();
```

<br>

# 浏览器嗅探
## User-Agent 检测
最早的浏览器嗅探即用户代理（user-agent）检测，服务端根据 user-agent 字符串来确定浏览器的类型。这个方法最大的问题是，解析 user-agent 字符串并非易事，由于浏览器为了确保其兼容性，都会复制另一个浏览器的用户代理字符串。因此随着每一个新浏览器的出现，用于用户代理检测的代码都需要更新，于是从新的浏览器发布，到代码更新并部署的这段时间便意味着不计其数的人会遭遇到糟糕的用户体验。

## 特性检测
特性检测的原理是为特定浏览器的特性进行测试，并仅当特性存在时即可应用特性检测。
```
// 不推荐的写法
if (navigator.userAgent.indexOf("MSIE 7") > -1) {
	// to do...
}

// 推荐的写法
if (document.getElementById) {
	// to do...
}
```
正确的特性检测的重要组成部分：
- 探测标准的方法；
- 探测不同浏览器的特性方法；
- 当被探测的方法均不存在时提供一个合乎逻辑的备用方法；

## 避免特性推断
一种不当的使用特性检测的情况是“特性推断”，特性推断尝试使用多个特性但仅验证了其中之一。
```
// 不推荐的写法
function getById (id) {
	var element = null;
	if(document.getElementByTagName) {  // DOM
		element = document.getElementById(id);
	} else if (window.ActiveXObject) {  // IE
		element = document.all[id];
	} else {  // Netscape <= 4
		element = document.layers[id];
	}

	return element;
}
```
总结：不能从一个特性的存在推断出另一个特性是否存在。

## 避免浏览器推断
