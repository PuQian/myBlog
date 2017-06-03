---
title: 【 Vue.js 】基础知识
date: 2016-12-20 16:29:03
tags: JavaScript
---

Vuejs是一套构建用户界面的渐进式框架，其目标是通过尽可能简单的 API 实现 **响应的数据绑定 ** 和 **组合的视图组件 **。

## Vue实例
### 构造器
实例化 Vue 时，需要传入一个 **选项对象**，它包含数据、模板、挂载元素、方法、生命周期钩子等选项。

### 属性与方法
每个 Vue 实例都会 **代理** 其 data 对象所有的属性，只有这些被代理的属性是响应的。
```
var data = {a : 1};
var vm = new Vue({
	el: "#example",
	data: data
});
```

### 实例生命周期
每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如，实例需要配置数据观测、编译模板、挂载实例到 DOM，然后在数据变化时更新 DOM。

## 模板语法
模板语法允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。

在底层的实现上， Vue 将模板编译成**虚拟 DOM 渲染函数**。结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。

### 插值
#### 文本
数据绑定最常见的 "Mustache" 双大括号语法
```
<span>Message:{{msg}}</span>
```

`v-once` 指令能够执行一次性插值
```
<span v-once>This will never change: {{msg}}</span>
```

#### 纯 HTML
双大括号将数据解释为纯文本，`v-html` 指令可以输出真正的 HTML
```
<div v-html="rawHTML"></div>
```
动态渲染任意的 HTML 会非常危险，因为容易导致 XSS 攻击。

#### 属性
双大括号不能在 HTML 属性中使用，应使用 `v-bind` 指令。
```
<div v-bind:id="dynamicId"></div>
```

#### 使用 JavaScript 表达式
表达式会在所属 Vue 实例作用域下作为 JavaScript 被解析，有个限制是只能绑定单个表达式。
```
{{ message.slice() }}
```

### 指令
指令的职责就是**当其表达式的值被改变时相应地将某些行为应用到   DOM 上**。

#### 过滤器
过滤器添加在 mustache 插值的尾部，过滤器函数总接受表达式的值作为第一个参数。
```
{{messgae | capitalize}}

new Vue({
	// ...
	filters: {
		capitalize: function(value) {
			if(!value) return '';
		}
	}
})
```

#### 缩写
Vue.js 为两个最为常用的指令提供了特别的缩写。

`v-bind` 缩写
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写-->
<a :href="url"></a>
```

`v-on` 缩写
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写-->
<a @click="doSomething"></a>
```

## 计算属性
任何复杂逻辑，都应当使用计算属性。

### 实例
```
<div id="example">
	<p>Original message: "{{message}}"</p>
	<p>Computed reversed message: "{{reversedMessage}}"</p>
</div>

var vm = new Vue({
	el: '#example',
	data: {
		message: 'Hello'
	},
	computed: {
		reversedMessage: function() {
			return this.message.split('').reverse().join('');
		}
	}
});
```
我们提供的函数将用作属性 vm.reversedMessage 的 getter，Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，依赖于 vm.reversedMessage 的绑定也会更新。

### 计算缓存 vs Methods
**计算属性是基于它的依赖缓存，计算属性只有在它的相关依赖发生改变时才会重新取值；而 methods 调用总会执行函数 **。

### 计算缓存 vs Watched Property
`watch` 方法用于观察 Vue 实例上的数据变动。不过通常更好的办法是使用计算属性而不是一个命令式的 watch 回调。但是 watch 可用于执行异步操作，能够在得到最终状态前，设置中间状态，这是计算属性无法做到的。
```
<div id="demo">{{fullName}}</div>

// watch 方法观察数据变动
var vm = new Vue({
	el: "#demo",
	data: {
		firstName: 'Foo',
		lastName: 'Bar',
		fullName: 'Foo Bar'
	},
	watch: {
		firstName: function(val) {
			this.fullName = val + ' ' + this.lastName;
		},
		lastName: function(val) {
			this.fullName = this.firstName + ' ' + val;
		}
	}
})

// computed 计算方法
var vm = new Vue({
	el: "#demo",
	data: {
		firstName: 'Foo',
		lastName: 'Bar'
	},
	computed: {
		fullName: function(){
			return this.firstName + ' ' + this.lastName;	
		}
	}
});
```

## 前端模块化

Vuejs是轻量级的MVVM框架，结合了React和Angular的优点，强调了React组件化的概念，实现数据和展现的分离以及Angular丰富的指令。

1. new 一个 vue 对象的时候你可以设置它的属性，其中最重要的包括三个，分别是 data，methods，watch，其中 data 代表 vue 对象的数据（Model 层），methods代表 vue对象的方法，watch 设置了对象监听的方法；
2. Vue对象里设置通过 **html 指令**与数据层交互，Model 层与 view 层交互；
3. 重要的HTML指令包括：v-text(渲染数据)、v-if（控制显示）、v-on（绑定事件）、v-for（循环渲染）等。

## Vue.js 实现双向绑定的原理
### 访问器属性
访问器属性是对象中的一种特殊属性，不能直接在对象中设置，必须通过 defineProperty() 方法单独定义。

```
// 定义名为 hello 的访问器属性
var obj = {};
Object.defineProperty(obj, "hello", {
  get: function(){
     console.log("get 方法被调用了" + this.name);  // this 执行 obj
  },
  set: function(val){
     console.log("set 方法被调用了，参数是：" + val);
  }
})
obj.hello;   // 读取访问器属性，调用 get 函数
obj.hello = "abc";   // 为属性赋值，调用 set 函数
```
 get和set方法内部的this都指向obj，这意味着get和set函数可以操作对象内部的值。另外，访问器属性的会"覆盖"同名的普通属性，因为访问器属性会被优先访问，与其同名的普通属性则会被忽略（也就是所谓的被"劫持"了）。

### 极简单响应式双向绑定的实现
```
<input type="text" id="a">
<span id="b"></span>

var obj = {};
Object.defineProperty(obj, "hello", {
	set: function(newVal) {
		document.getElementById("a").value = newVal;
		document.getElementById("b").innerHTML = newVal;
	}
})
document.addEventListener('keyup', function(e){
	obj.hello = e.target.value;
})
```
