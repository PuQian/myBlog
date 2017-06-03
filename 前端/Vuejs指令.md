---
title: 【 Vue.js 】指令
date: 2016-12-22 16:52:34
tags: JavaScript
---
# Class 与 Style 绑定

## 绑定 HTML Class
### 对象语法
```
<div class="static" v-bind:class="{active: isActive}"></div>

data: {
	isActive: true
}

<!-- 渲染为 -->
<div class="static isActive"></div>
```

### 数组语法
```
<div v-bind:class="[activeClass, errorClass]"></div>

data: {
	activeClass: 'active',
	errorClass: 'text-danger'
}

<!-- 渲染为-->
<div v-bind:class="active text-danger"></div>
```

### 组件用法
```
// 声明组件
Vue.component('my-component', {
	template: '<p class="foo bar">Hi</p>'
});

// 使用组件
<my-component class="baz boo"></my-component>

<!-- 渲染为 -->
<p class="foo bar baz boo"></p>
```

## 绑定内联样式
### 对象语法
```
<div v-bind:style="{color: activeColor, fontSize: fontSize + 'px'}"></div>

data: {
	activeColor: 'red',
	fontSize: 30
}
```

### 数组语法
可以将多个样式对象应用到一个元素上
```
<div v-bind:style="[baseStyles, overrideStyles]"></div>
```

# 条件渲染
## v-if
```
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

## template 中 v-if 条件组
改变多个元素的显示情况是使用 template 元素吗。把一个 template 元素当做包装元素，并在上面使用 v-if，最终的渲染结果不会包含它。
```
<template v-if="ok">
	<h1>Title</h1>
	<p>Paragraph 1</p>
</template>
```