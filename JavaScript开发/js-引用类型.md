---
title: 【 JS 】 引用类型
date: 2017-01-25 12:35:14
tags: JS
---
# Object 类型
## 访问对象中属性的两种方法比较
1.点表示法：person.name
2.方括号表示法：person["name"]
比较：1.功能上看没有区别，但是方括号语法的优点是可以通过变量来访问属性，例如：`var propertyName = "name"; person[propertyName]`；
2.属性名中包含会导致错误的语法的字符，或者属性名使用的是**关键字或保留字**，也可以使用方括号表示法。例如：person["first name"]
3.通常，除非必须使用变量访问属性，否则建议使用点表示法。

# Array 类型
## 检测数组
**Array.isArray()**：检测数组，支持浏览器：IE9+、Firefox 4+、Safari 5+、Opera 10.5+ 和 Chrome。

## 转换方法
**toString()/toLocalString()/valueOf()**：默认情况下以逗号分隔的字符串的形式返回数组项；

**join()**：使用不同的分隔符来构建数组组成的字符串，只接收一个参数，即用作分隔符的字符串；不传递参数默认以逗号分隔。

## 栈方法
**push()/pop()**：栈方法，数组表现如栈一样，先进后出。
push()：将几个数组项推入数组的末尾；
pop()：从数组的末尾弹出一项；
```
var colors = ["red", "blue"];
colors.push("brown");   // 添加一项
colors[3] = "black";    // 添加另一项
alert(colors.length);   // 4

var item = colors.pop();
alert(item);     // "black"
```

## 队列方法
**shift()/unshift()**：队列方法，数组表现如队列一样，先进先出。
shift()：移除数组中的第一项并返回该项，同时数组长度减1；
unshift()：在数组前端推入几项并返回新数组的长度；
```
var colors = Array();  // 省略 new
var count = colors.unshift("red", "green", "black");  // 推入三项
alert(count);    // 3

var item = colors.pop();   // 取出最后一项
alert(item);    // black
```

## 重排序方法
**reverse()**：反转数组项的顺序；
```
var values = [1,2,3,4,5];
values.reserve();
alert(values);   // 5,4,3,2,1
```

**sort()**：默认情况下按升序排列数组项，sort()方法**比较的是字符串**；
```
// 按字符串比较
var values = [0,1,5,10,15];
values.sort();
alert(vaules);  // 0,1,10,15,5

// 改进：向 sort() 函数传入参数，规定比较规则
function compare(value1, value2){
	if(value1 < value2) return -1;   // 升序排序
	else if(value1 > value2) return 1;
	else return 0;
}
var values = [0,1,5,10,15];
values.sort(compare);
alert(values);   // 0,1,5,10,15
```

## 操作方法
**concat()**：拼接数组。基于当前数组中的所有项创建一个新数组，然后将接收到的参数添加到副本的末尾，返回新数组。
```
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);

alert(colors);  // red,green,blue
alert(colors2);  // red,green,blue,yellow,black,brown
```

**slice()**：选择数组中的几项，接收一或两个参数。只有一个参数的情况下，slice()返回从该参数指定位置开始到当前数组末尾的所有项；如果有两个参数，返回开始和结束位置（不包括结束位置）之间的项。注意，slice()方法不会影响原始数组。
```
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);  
var colors3 = colors.slice(1,4);

alert(colors2);   // green,blue,yellow,purple
alert(colors3);   // green,blue,yellow
```

**splice()**：向数组的中部插入项，使用方式有以下三种：
删除：可以删除任意数量的项，**指定2个参数即为删除**，参数为要删除的第一项的位置和要删除的项数；
插入（或替换）：可以向指定位置插入（或替换）任意数量的项，**指定3个参数即为插入**，参数为起始位置、要删除的项数（可以为0）、要插入的项（可以超过2个）；
```
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1);   // 删除第一项
alert(colors);   // green,blue
alert(removed);  // red

removed = colors.splice(1, 0, "yellow", "orange");  // 从位置1开始插入两项
alert(colors);   // green,yellow,orange,blue
alert(removed);  // 返回是一个空数组

removed = colors.splice(1, 1, "red", "purple");  // 插入两项，删除一项
alert(colors);   // green,red,purple,orange,blue   
alert(removed);  // yellow  
```

## 位置方法
**indexOf()/lastIndexOf()**：接收两个参数，要查找的项和（可选的）表示查找起点位置的索引，返回要查找的项在数组中的位置，没有找到返回-1。支持的浏览器：IE9+、Firefox 2+、Safari 3+、Opera 9.5+ 和  Chrome。
```
var numbers = [1,2,3,4,5,4,3,2,1];

alert(numbers.indexOf(4));     // 3
alert(nmbers.lastIndexOf(4));  // 5 

alert(numbers.indexOf(4,4));      // 5
alert(numbers.lastIndexOf(4,4));  // 3
```

## 迭代方法
ECMAScript5 为数组定义了 5 个迭代方法。每个方法接收两个参数：要在每一项运行的函数 和 （可选的）运行该函数的作用域对象——影响 this 的值。传入这些方法中的函数会接收三个参数:数 组项的值、该项在数组中的位置和数组对象本身。
**这五个方法都是对数组中的每一项运行给定函数，只是返回值不同。**
**every()**：如果函数对每一项（全部项）都返回true，则返回true；
**filter()**：返回该函数会返回 true 的项组成的数组；
**forEach()**：方法没有返回值；
**map()**：返回每次函数调用的结果组成的数组；
**some()**：如果该函数对任一项（存在一项）返回true，则返回true。

```
var numbers = [1,2,3,4,5,4,3,2,1];

// every()
var everyResult = numbers.every(function(item, index, array){
	return (item > 2);
});
alert(everyResult);   // false

// some()
var someResult = numbers.some(function(item, index, array){
         return (item > 2);
});
alert(someResult);   //true

// filter()
var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});
alert(filterResult);   //[3,4,5,4,3]

// map()
var mapResult = numbers.map(function(item, index, array){
    return item * 2;
});
alert(mapResult);  //[2,4,6,8,10,8,6,4,2]

// forEach()
numbers.forEach(function(item, index, array){
	// 执行某些操作
});
```
支持这些迭代方法的浏览器有 IE9+、Firefox 2+、Safari 3+、Opera 9.5+和 Chrome。

## 归并方法
reduce() 和 reduceRight()，迭代数组的所有项，构建一个最终返回的值。传入函数接收 4 个参数：前一个值、当前值、项的索引和数组对象。
```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
	return prev + cur;
});
alert(sum);  // 15
```


# Function 类型
## 函数返回值
1. 无需指定函数的返回值，因为任何函数都可以在任何时候返回任何值；
2. 没有指定返回值的函数返回的是一个特殊的 undefined 值；
3. 不存在函数签名的特性，因此函数不能重载。

## 函数中的参数
1.ECMAScript 函数不介意传递进来多少个参数，也不在乎传进来的参数是什么数据类型。原因是EMAScript中的**参数内部是用一个数组表示的**，函数接收到的始终是这个数组，而不关心数组中包含哪些参数。

```
function howManyArgs(){
	alert(arguments.length);
	// 访问：arguments[0]
}
howManyArgs();   // 0
howManyArgs(12); // 1
howManyArgs("string", 34)  // 2
```

2.传递参数，JS中所有函数的参数都是按值传递的。在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量；在向参数传递引用类型的值时，会把这个值在内存中的地址复制一个给局部变量，因此这个局部变量的变化会反映在函数的外部。

```
// 传递引用类型的值，会概不函数外部对象
function setName(obj){
	obj.name = "Alice";
}
var person = new Object();
setName(person);
alert(person.name);   // Alice


// 传递引用类型的值也是按值传递的
function setName(obj){
	obj.name = "Alice";
	obj = new Object();
	obj.name = "Bob";
}
var person = new Object();
setName(person);
alert(person.name);   // Alice
```

## 函数声明与函数表达式
解析器会**率先读取函数声明**，并使其在执行任何代码之前可用；函数表达式则等到解析器执行到其所在的代码航，才会真正被解释执行。
```
// 函数声明实例
alert(sum(10,10));   //20
function sum(num1, num2){
	return num1 + num2;
}

// 函数表达式
alert(sum(10,10));   // 发生错误
var sum = function(num1, num2){
	return num1 + num2;
}
```

## 函数内部属性
函数内部有两个特殊的对象：arguments 和 this。
### arguments.callee
保存拥有这个 arguments 对象的函数，主要用于在递归算法中使用。
```
// 求阶乘
function factorial(num) {
	if(num <= 1){
		return 1;
	} else {
		return num * arguments.callee(num - 1);
	}
}
```

### this
this 引用的是**函数据以执行的环境对象**。

### caller 属性
caller 属性保存着调用当前函数的函数的引用，如果在全局作用域中调用当前函数，其值为 null。
```
// caller 实例
function outer() {
	inner();
}
function inner() {
	alert(arguments.callee.caller);   // 输出 outer 函数源代码
}
outer();
```

## 函数属性和方法
每个函数都包含两个属性：length 和 prototype。其中，length 属性表示**函数希望接收的命名参数的个数**。prototype是保存**所有**实例方法的真正所在，在创建自定义引用类型以及实现继承时，prototype属性的作用是极为重要的。

### apply() 和 call()
每个函数都包含两个非继承而来的方法：apply() 和 call()。这两个方法的用途都是**在特定的作用域中调用函数**，实际上等于设置函数体内 this 对象的值。
**apply()**方法接收两个参数:一个 是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 Array 的实例，也可以是 arguments 对象。
```
function sum(num1, num2) {
	return num1 + num2;
}

function callSum1(num1, num2) {
	return sum.apply(this, arguments);   // 传入 arguments 对象
}

function callSum2(num1, num2) {
	return sum.apply(this, [num1, num2]);  // 传入参数数组
}

alert(callSum1(10,10));  // 20
alert(callSum1(10,10));  // 20
```

**call()**方法与 apply()方法的作用相同，它们的区别仅在于**接收参数的方式不同**。对于 call() 方法而言，第一个参数是 this 值没有变，变化的是**其余参数都直接传递给函数**。换句话说，使用 call() 方法时，**传递给函数的参数必须逐个列举出来**。
```
function sum(num1, num2){
	return num1 + num2;
}

function callSum(num1, num2){
	return sum.call(this, num1, num2);
}

alert(callSum(10,10));   // 20
```

事实上,传递参数并非 apply()和 call()真正的用武之地；它们真正强大的地方是**能够扩充函数赖以运行的作用域**。其好处是：对象不需要与方法存在任何耦合关系。
```
window.color = "red";
var o = { color: "blue" };

// 在全局定义的函数，默认情况下 this 指向 window
function sayColor(){
    alert(this.color);
}

sayColor();             // red
sayColor.call(this);    // red
sayColor.call(window);  // red
sayColor.call(o);       // blue
```

### bind()
创建一个函数的实例，其 this 值会被绑定到传给 bind()函数的值。
```
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    alert(this.color);
}

var objectSayColor = sayColor.bind(o);
objectSayColor();    // blue
```
支持 bind()方法的浏览器有 IE9+、Firefox 4+、Safari 5.1+、Opera 12+ 和 Chrome。

# 问题
##介绍js有哪些内置对象？
> 
Object 是 JavaScript 中所有对象的父对象
>
数据封装类对象：Object、Array、Boolean、Number 和 String
其他对象：Function、Arguments、Math、Date、RegExp、Error
> 
参考：http://www.ibm.com/developerworks/cn/web/wa-objectsinjs-v1b/index.html  

## this对象的理解
- this总是指向函数的直接调用者（而非间接调用者）；
- 如果有new关键字，this 指向 new 出来的那个对象；
- 在事件中，this 指向触发这个事件的对象，特殊的是，IE 中的 attachEvent 中的 this 总是指向全局对象 Window；

##["1", "2", "3"].map(parseInt) 答案是多少？##
		
parseInt() 函数能解析一个字符串，并返回一个整数，需要两个参数 (val, radix)，
其中 radix 表示要解析的数字的基数。【该值介于 2 ~ 36 之间，并且字符串中的数字不能大于radix才能正确返回数字结果值】;
但此处 map 传了 3 个 (element, index, array),我们重写parseInt函数测试一下是否符合上面的规则。

function parseInt(str, radix) {   
    return str+'-'+radix;   
};  
var a=["1", "2", "3"];  
a.map(parseInt);  // ["1-0", "2-1", "3-2"] 不能大于radix

因为二进制里面，没有数字3,导致出现超范围的radix赋值和不合法的进制解析，才会返回NaN
所以["1", "2", "3"].map(parseInt) 答案也就是：[1, NaN, NaN]

详细解析：http://blog.csdn.net/justjavac/article/details/19473199

**.call() 和 .apply() 的区别？**


> 例子中用 add 来替换 sub，add.call(sub,3,1) == add(3,1) ，所以运行结果为：alert(4);

注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。

```
function add(a,b)
{
    alert(a+b);
}

function sub(a,b)
{
    alert(a-b);
}

add.call(sub,3,1);
```

<br>













