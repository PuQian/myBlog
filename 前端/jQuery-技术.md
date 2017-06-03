---
title: 【 jQuery 】方法汇总
date: 2016-03-23 17:27:07
tags: jQuery
---
# jQuery 简介
## 安装
### 从 CDN 中载入 jQuery
```
--- 百度 CDN ---
<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js">

--- 新浪 CDN ---
<script src="http://lib.sinaapp.com/js/jquery/2.0.2/jquery-2.0.2.min.js">
```
<br/>

**使用百度、又拍云、新浪、谷歌或微软的 jQuery，有一个很大的优势：**

许多用户在访问其他站点时，已经从百度、又拍云、新浪、谷歌或微软加载过 jQuery。所有结果是，当他们访问您的站点时，会从缓存中加载 jQuery，这样可以减少加载时间。同时，大多数 CDN 都可以确保当用户向其请求文件时，会从离用户最近的服务器上返回响应，这样也可以提高加载速度。

<br/>

## 语法
### jQuery 选择器
```
$("*")                     选取所有元素
$(this)                    选取当前 HTML 元素
$("p.intro")               选取 class 为 intro 的 <p> 元素
$("p:first")               选取第一个 <p> 元素
$("ul li:first")           选取第一个 <ul> 元素的第一个 <li> 元素
$("ul li:first-child")     选取每个 <ul> 元素的第一个 <li> 元素
$("[href]")                选取带有 href 属性的元素
$("a[target='_blank']")	   选取所有 target 属性值等于 "_blank" 的 <a> 元素
$("a[target!='_blank']")   选取所有 target 属性值不等于 "_blank" 的 <a> 元素
$(":button")               选取所有 type="button" 的 <input> 元素 和 <button> 元素
$("tr:even")               选取偶数位置的 <tr> 元素
$("tr:odd")                选取奇数位置的 <tr> 元素
```

<br/>

### jQuery 事件
```
|  鼠标事件    |   键盘事件 |   表单事件  |   文档/窗口事件  | 
|  click      |  keypress  |  submit    |      load       |
|  dblclick   |  keydown   |  change    |     resize      |
|  mouseenter |  keyup     |  focus     |     scroll      |
|  mouseleave |  blur      |  unload    |
|  mousedown  |
|  mouseup    |
|  hover      |
|  blur       |
```

<br/>

## jQuery 效果

### 隐藏显示
```
show(speed, callback)    显示
hide(speed, callback)    隐藏
toggle(speed, callback)  显示被隐藏的元素，隐藏已显示的元素
```

<br/>

### 淡入淡出
```
fadeIn(speed, callback)           淡入已隐藏的元素
fadeOut(speed, callback)          淡出可见元素
fadeToggle(speed, callback)       切换淡入淡出效果
fadeTo(speed, opacity, callback)  渐变为给定的不透明度
```

<br/>

### 滑动
```
slideDown(speed,callback)     向下滑动元素
slideUp(speed,callback)       向上滑动元素
slideToggle(speed,callback)   切换滑动效果
```

<br/>

### 动画
`animate({params},speed,callback) `

1. 必需属性：params 为必需属性，定义形成动画的 CSS 属性；

2. Camel标记法：animate() 方法几乎可以操作所有的 CSS 属性，必须使用 Camel 标记法书写所有的属性名；
   例如：paddingLeft、marginRight 等，色彩动画不包含在核心 jQuery 库中。

3. 使用相对值：可以定义相对于元素当前值的相对值，使用 += 或 -=；例如：height : '+= 150px'。

4. 使用预定义的值：将属性动画设置为预定值 "show"、"hide"、"toggle"；例如： height:'toggle'。

5. 队列功能：编写多个 animate() 调用，jQuery 会创建包含这些方法调用的 "内部" 队列，逐一运行这些 animate 调用。


<br/>

### 停止动画

`stop(stopAll,goToEnd)`

1. stop() 方法用于在动画完成之前停止动画或效果，适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。
2. stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
3. goToEnd 参数规定是否立即完成当前动画。默认是 false。


<br/>

### jQuery 链
在相同的元素上运行多条 jQuery 命令，命令逐条执行，例如：
```
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
```

## jQuery HTML
### jQuery 捕获

获得内容：`text()`、`html()`、`val()`
获取属性：`attr()` 