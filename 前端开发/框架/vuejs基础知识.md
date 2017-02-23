---
title: 【 Vue.js 】基础知识
date: 2017-01-07 16:29:03
tags: JavaScript
---
**前端模块化**

Vuejs是轻量级的MVVM框架，结合了React和Angular的优点，强调了React组件化的概念，实现数据和展现的分离以及Angular丰富的指令。

1. new 一个 vue 对象的时候你可以设置它的属性，其中最重要的包括三个，分别是 data，methods，watch，其中 data 代表 vue 对象的数据（Model 层），methods代表 vue对象的方法，watch 设置了对象监听的方法；
2. Vue对象里设置通过 **html 指令**与数据层交互，Model 层与 view 层交互；
3. 重要的HTML指令包括：v-text(渲染数据)、v-if（控制显示）、v-on（绑定事件）、v-for（循环渲染）等。

重要概念：
1. 双向绑定

# 如何划分组件
功能模块 - select、pagenation ……
页面区域 - header、footer、sidebar ……