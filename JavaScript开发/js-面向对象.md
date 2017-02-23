---
title: 【 JS 】 面向对象
date: 2016-12-15 23:07:22
tags: JS
---

# 问题

## JavaScript继承的几种实现方式？
参考：[构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)，
[非构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)；

1、构造继承
2、原型继承
3、实例继承
4、拷贝继承

原型prototype机制或apply和call方法去实现较简单，建议使用构造函数与原型混合方式。
```
function Parent(){
	this.name = 'wang';
}

function Child(){
	this.age = 28;
}
Child.prototype = new Parent();//继承了Parent，通过原型

var demo = new Child();
alert(demo.age);
alert(demo.name);//得到被继承的属性

```

## JavaScript原型，原型链? 有什么特点？

每个对象都会在其内部初始化一个属性，就是prototype(原型)，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，**这个 prototype 又会有自己的 prototype**，
于是就这样一直找下去，也就是我们平时所说的原型链的概念。
关系：instance.constructor.prototype = instance.__proto__

特点：
JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

当我们需要一个属性的时，Javascript引擎会先看当前对象中是否有这个属性， 如果没有的话，就会查找他的Prototype对象是否有这个属性，如此递推下去，一直检索到 Object 内建对象。
```
function Func(){}
Func.prototype.name = "Sean";
Func.prototype.getInfo = function() {
  return this.name;
}
var person = new Func();       //现在可以参考var person = Object.create(oldObject);
console.log(person.getInfo()); //它拥有了Func的属性和方法
//"Sean"
console.log(Func.prototype);
// Func { name="Sean", getInfo=function()}
```

## javascript创建对象的几种方式？

**1、对象字面量的方式** 
```
person = {
	firstname : "Mark",
	lastname  : "Yun",
    age       : 25,
	eyecolor  : "black"
};
```

**2、自定义构造函数模式（无参，使用 function 模拟构造函数）**
```
function Person(){}
var person = new Person();  // !! 自定义构造函数（new 关键字）

person.name = "Mark";
person.age = "25";
person.work = function(){
	alert(person.name + " hello...");
}
person.work();
```

**3、自定义构造函数模式（有参，使用 function 模拟构造函数）**
```
// 定义用于创建对象的构造函数
function Pet(name,age,hobby){
   this.name = name;   //this作用域：当前对象
   this.age = age;
   this.hobby = hobby;
   this.eat = function(){
      alert("我叫"+this.name+");
   }
}
var maidou = new Pet("麦兜",25,"coding");  //实例化、创建对象
maidou.eat();   //调用eat方法
```
**自定义构造函数的特点**：
(1)没有显式地创建对象；(2)直接将属性和方法赋给了 this 对象；(3)没有 return 语句。

**new 操作符执行的操作**：
(1) 创建一个新对象； (2) 将构造函数的作用域赋给新对象(因此 this 就指向了这个新对象)； (3) 执行构造函数中的代码(为这个新对象添加属性)；(4) 返回新对象。

**4、工厂模式（内置对象）**
```
function createPerson(name, age, job) {
	var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };
	return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```
调用 createPerson 函数每次都返回一个对象，解决了创建多个相似对象的问题。
缺点是：没有解决对象的识别问题。

**5、用原型方式来创建**
```
function Dog(){}

Dog.prototype.name = "旺财";
Dog.prototype.eat = function(){
	alert(this.name+"是个吃货");
}

var wangcai = new Dog();
wangcai.eat();
```

**5、用混合方式来创建**
```
function Car(name,price){
	this.name = name;
	this.price = price; 
}
Car.prototype.sell = function(){
	alert("我是"+this.name+");
}

var camry = new Car("凯美瑞",27);
camry.sell(); 
```

<br>

## new操作符具体干了什么呢?

> 1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
2、属性和方法被加入到 this 引用的对象中。
3、新创建的对象由 this 所引用，并且最后隐式的返回 this 。

```
var obj  = {};
obj.__proto__ = Base.prototype;
Base.call(obj);
```

## 下面程序执行后弹出什么样的结果?

```
function fn() {
    this.a = 0;
    this.b = function() {
        alert(this.a)
    }
}
fn.prototype = {
    b: function() {
        this.a = 20;
        alert(this.a);
    },
    c: function() {
        this.a = 30;
        alert(this.a);
    }
}
var myfn = new fn();
myfn.b();
myfn.c();
```

答案：0、30


