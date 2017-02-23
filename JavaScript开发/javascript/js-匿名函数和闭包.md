---
title: 【 JS 】匿名函数和闭包
date: 2016-06-04 22:23:57
tags: JavaScript
---
# 闭包
## 变量的作用域

变量的作用域有两种：全局变量和局部变量。函数内部可以直接读取全局变量，在函数外部不能读取函数内部的局部变量。注意：**函数内部声明变量时，一定要使用 var 命令，如果不用的话，实际上声明的是一个全局变量**。

<br>

## 如何从外部读取局部变量
出于种种原因，有时需要得到函数内部的局部变量，实现的方法就是 **在函数内部再定义一个函数**。

```
function f1(){
	var n = 99;
	function f2(){
		alert(n);  // 99
	}
}
```

js语言特有的**链式作用域结构**：子对象会一级一级地向上寻找父对象的变量。因此父对象的所有变量对子对象都是可见的，反之不成立。

函数 f2 可以读取 f1 中的局部变量，那么只要把 f2 作为返回值，就可以在 f1 外部读取它的内部变量了。

```
function f1(){
	var n = 99;
	function f2(){
		alert(n);
	}
	return f2;
}
var result = f1();

result();   // 99
```

<br>

## 闭包的概念
上面的代码中的 f2 函数就是闭包，**闭包就是可以读取其他函数内部变量的函数**。由于在 js 语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成 **定义在一个函数内部的函数**。

<br>

## 闭包的用途
闭包的用途有两个，一个是可以 **读取函数内部的变量**，另一个就是 **让这些变量的值始终保存在内存中**。

```
function f1(){
	var n = 99;
	nAdd = function(){n++;}  // 全局变量 + 匿名函数 + 闭包
	function f2(){
		alert(n);
	}
	return f2;
}
var result = f1();  // result = f2;
result();   // 99
nAdd();     // n =100;
result();   // 100
```

函数 f1 的局部变量一直保存在内存中，并没有在 f1 调用后被自动清除。原因是 f1 是 f2 的父函数，而 f2 被赋予给了一个全局变量，这导致 f2 始终在内存中，而 f2 的存在依赖 f1，因此 f1 也始终在内存中，不会在调用结束后被垃圾回收机制回收。

代码中还有一点需要注意的是：nAdd是一个全局变量，而不是局部变量；其次，nAdd的值是一个匿名函数，而这个匿名函数本身也是一个闭包；因此，nAdd 相当于一个 setter，可以在函数外部对函数内部的局部变量进行操作。

<br>

## 使用闭包的注意点

### 内存泄漏
由于闭包会使得函数中的变量都保存在内存中，**内存消耗很大**，所以不能滥用闭包，否则会造成网页的性能问题，在 IE 中可能导致内存泄漏，IE 的 JScript 对象和 DOM 对象使用不同的垃圾收集方式。**解决方法是：在退出函数之前，将不使用的局部变量删除**，如果没有解除引用，需要等到浏览器关闭才得以释放。 


```
// 内存泄漏实例
function box(){
	var oDiv = document.getElementById('oDiv'); 
	oDiv.onclick = function(){
		alert(oDiv.innerHTML);  // 这里使用 oDiv 导致内存泄漏，oDiv 会一直驻留在内存中
	}
}

// 解决方法
function box(){
	var oDiv = document.getElementById('oDiv'); 
	var text = oDiv.innerHTML;
	oDiv.onclick = function(){
		alert(text); 
	}
	oDiv = null;  // 解除引用
}
```

<br>

### 循环里的匿名函数

由于闭包里作用域返回的局部变量资源不会被立刻撤销回收，作用域链的机制导致一个问题，在循环里的匿名函数取得的任何变量都是最后一个值。

```
// 循环里包含匿名函数
function box(){
	var arr = [];
	for (var i = 0; i < 5; i++) {
		arr[i] = function(){
			return i;
		};
	}
	return arr;   // 返回函数数组
}

var b = box();   // 得到函数数组
for(var i = 0; i < b.length; i++) {
	alert(b[i]());   // 输出每个函数的值都是 5
}

// 改进方法：使用自我执行匿名函数
// 1. 返回数组
function box(){
	var arr = [];
	for(var i = 0; i < 5; i++){
		arr[i] = (function(num){ 
			return num;
		})(i);  // 自我执行并传参
	}
	return arr;   // 返回数组而不是函数数组
}

var b = box();
for(var i = 0; i < b.length; i++){
	alert(b[i]);
}

// 2. 返回函数数组 
function box(){
	var arr = [];
	for(var i = 0; i < 5; i++){
		arr[i] = function(num){
			return function(){
				return num;
			}
		}(i);
	}
	return arr;  // 返回函数数组
}
var b = box();
for(var i = 0; i < b.length; i++){
	alert(b[i]());  
}
```

<br>

### this 对象
**this 对象是在运行时基于函数的执行环境绑定的**。如果 this 在全局范围，this 就是 window；如果 this 在对象内部，就指向这个对象。闭包在运行时指向 window，因此闭包并不属于这个对象的属性或方法。

```
var user = "The Window";
var obj = {
	user : 'The Object',
	getUserFunction : function(){
		return function(){  // 闭包不属于 obj，里面的 this 指向 window
			return this.user;
		};
	}
};
alert(obj.getUserFunction()());  // The Window

// 解决方法一：强制指向某个对象
alert(obj.getUserFunction().call(obj));

// 解决方法二：从上一个作用域中得到对象
getUserFunction : function () {  
	var that = this; //从对象的方法里得对象  
	return function () {  
    	return that.user;  
  	};
}
```

<br>

# 匿名函数
匿名函数就是没有名字的函数。

```
// 通过表达式自我执行
(function(age){     // 封装成表达式
	alert(age);  // 100
})(100);

// 函数里的匿名函数
function box(){
	return function(){  // 匿名函数 + 闭包
		return 'Lee';
	}
}
alert(box()());   // 输出 Lee
```

<br>

# 问题
**什么是闭包（closure），为什么要用它？**
> 
闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。

闭包的特性：
1.函数内再嵌套函数
2.内部函数可以引用外层的参数和变量
3.参数和变量不会被垃圾回收机制回收

```
// li 节点的 onclick 事件都能正确的弹出当前被点击的 li 索引
 <ul id="testUL">
    <li> index = 0</li>
    <li> index = 1</li>
    <li> index = 2</li>
    <li> index = 3</li>
</ul>
<script type="text/javascript">
  	var nodes = document.getElementsByTagName("li");
	for(i = 0; i < nodes.length; i += 1){
	    nodes[i].onclick = (function(num){
	            return function() {
	                console.log(num);
	            } //不用闭包的话，值每次都是4
	        })(i);
	}
</script>
```

下面的代码，执行 say667() 后，say667()闭包内部变量会存在，而闭包内部函数的内部变量不会存在
使得 Javascript 的垃圾回收机制GC不会收回 say667() 所占用的资源
因为 say667() 的内部函数的执行需要依赖 say667() 中的变量，这是对闭包作用的非常直白的描述。

```
function say667() {
	// Local variable that ends up within closure
	var num = 666;
	var sayAlert = function() {
		alert(num);
	}
	num++;
	return sayAlert;
}

var sayAlert = say667();
sayAlert();  //执行结果应该弹出的667
```
<br>

```
function fun(n, o){
	console.log(o);
	return {
		fun : function(m){
			return fun(m,n);
		}
	}
}
var a = fun(0); a.fun(1); a.fun(2); a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1); c.fun(2); c.fun(3);
```

第一行：第一次调用 fun(0) 时，o 为 undefined；
第二次调用 fun(1) 时，m 为 1，fun 闭包了外层函数的 n，也就是第一次调用的 n = 0，即 m = 1，n = 0，并在内部调用 fun(1,0); 输出 o = 0；
第三次调用 fun(2) 时，m 为 2，fun 闭包的还是第一次调用时的 n，所以调用 fun(2,0)，o 为 0。
最终答案是：undefined、0、0、0。

第二行：第一次调用 fun(0)，o 为 undefined；
第二次调用 fun(1) 时，m 为 1，fun 闭包了外层函数的 n，也就是第一次调用的 n = 0，fun(1,0)，输出 o = 0；
第三次调用 fun(2) 时，m 为 2，fun 闭包了第二次调用的 fun 函数中的n，n = 1，fun(2,1)，o 为 1；
最终答案是：undefined、0、1、2。

第三行最终答案是：undefined、0、1、1；

fun(0) = fun(0,undefined) = {fun: function(m){return fun(m,0)}};
fun(0).fun(1) = fun(1,0) = {fun: function(m){return fun(m,1)}};
fun(0).fun(1).fun(2) = {fun: function(m){return fun(m,2)}};
...

<br>

参考资料：[滕柳的博客](http://blog.csdn.net/tengliu6/article/details/52571681) | [CSDN总结](http://lib.csdn.net/article/javascript/58698) | [小小沧海](http://www.cnblogs.com/xxcanghai/p/4991870.html) | [经典问题](https://segmentfault.com/a/1190000003818163)























