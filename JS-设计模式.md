---
title: 【 JS 】设计模式
date: 2016-02-18 21:42:18
tags: JavaScript
---
# 问题
## 说说你对 MVC 和 MVVM 的理解
### MVC
View 传送指令到 Controller，Controller 完成业务逻辑后，要求 Model 改变状态，Model 将新的数据发送到 View，用户得到反馈，**所有通信都是单向的**。

### MVVM
Angular它采用**双向绑定**（data-binding）：**View的变动，自动反映在 ViewModel，反之亦然**。

组成部 分Model、View、ViewModel
View：UI界面（DOM）
ViewModel：它是View的抽象，负责View与Model之间信息转换，将View的Command传送到Model；
Model：数据访问层（JS对象）

VueJS、React、Angular 都是 MVVM 框架

## 对 vue.js 的理解
1. 它是一个轻量级的 MVVM 框架；
2. 数据驱动 + 组件化的前端开发；
3. Github 非常高的 star 数，社区完善；

### 核心思想
1.数据驱动：**DOM 是数据的一种自然映射**；数据响应原理是数据（model）改变驱动视图（view）自动更新；
2.组件化：扩展 HTML 匀速，封装可重用的代码；
组件设计原则：页面上每个独立的可视/可交互区域视为一个组件；每个组件对应一个工程目录，组件所需要的各种资源在这个目录下就近维护；页面不过是组件的容器，组件可以嵌套自由组合形成完整的页面；



## Augular、React 和 Vue 对比
1. Vue.js 更轻量，gzip 后大小只有 20K+；
2. Vue.js 更易上手，学习曲线平稳；
3. 吸取两家之长，借鉴了 angular 的指令和 react 的组件化；

## 说说你对AMD和Commonjs的理解

CommonJS是服务器端模块的规范，Node.js采用了这个规范。CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。

AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的。


