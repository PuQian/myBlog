---
title: 【 HTML 】元素
date: 2016-08-11 20:04:31
tags: HTML
---
# 块级元素

常见的块级元素有：

**标题**：h1、h2、h3、h4、h5、h6
**列表**：ul、ol、li、dl
**表格**：table、
**表单**：form、fieldset
**其他**：div、p、pre、blockquote、hr（水平分割线）、menu

# 内联（行内）元素
行内元素没有独立空间，对行内元素设置 **高度、宽度、外上下边距** 等属性是无效的，可以设置  **内边距 ** 及 **左右外边距** 属性。

**常用**：a、img、span、input、textarea、code
**样式**：i（斜体）、em（强调）、strong（粗体强调）、strike（中划线）、sub（下标）、sup（上标）
**其他**：br、select

# HTML5 新元素
## 新的语义/结构元素
`<header>` --- 文档或节的页眉、`<footer>` --- 文档或节的页脚、`<nav>` --- 文档内的导航链接
`<main>` --- 文档的主内容、`<section>` --- 文档中的节、`<article>` --- 文档内的文章
`<aside>` --- 页面内容之外的内容、`<dialog>` --- 对话框或窗口

`<details>` --- 用户可查看或隐藏的额外细节、`<summary>` ---  `<detail>` 元素的可见标题、
`<time>` --- 日期/时间、`<progress>` --- 任务进度。

## HTML5 图像
`<canvas>` --- 使用 JavaScript 的图像绘制、`<svg>` --- 使用 SVG 的图像绘制。

## 新的媒介元素
`<audio>` --- 声音或音乐内容。
`<embed>` --- 外部应用程序的容器（比如插件）。
`<source>` ---  `<video>` 和 `<audio>` 的来源。
`<track>` ---  `<video>` 和 `<audio>` 的轨道。
`<video>` --- 视频或影片内容。

# 问题
## title 和 h1 的区别
突出**网站标题或关键字**用title，突出**文章主题**用h1。从 SEO 角度看，title 的权重高于 h1，其适用性也更广。

## b 和 strong 的区别
b 是视觉要素，目的仅是加粗显示文本，是**无意义的加粗**；
strong 标签是表达要素，表示**强调文本**，提醒读者或终端注意，浏览器等终端将其加粗显示。
总结：`<b>` 是为了加粗而加粗是一种样式，`<strong>` 加粗是为了标明重点表示强调。另，盲人在使用阅读设备阅读网络时，strong 会重读，b 则不会；从网站优化方面分析，strong内的内容更容易被抓取到。

## i 和 em的区别
i 标签是视觉要素，表示**无意义的斜体**；
em 标签是表达要素，表示一般的强调文本。

strong 一般表示 html 页面上的强调，em 表示句子中的强调。

## html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和HTML5？

- HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
- 绘画 canvas
- 用于媒介回放的 video 和 audio 元素
- 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
- sessionStorage 的数据在浏览器关闭后自动删除
- 语意化更好的内容元素，比如 article、footer、header、nav、section
- 表单控件，calendar、date、time、email、url、search
- 新的技术webworker, websockt, Geolocation 
- 移除的元素
- 纯表现的元素：basefont，big，center，font, s，strike，tt，u；
- 对可用性产生负面影响的元素：frame，frameset，noframes；
- 支持HTML5新标签：
- IE8/IE7/IE6支持通过document.createElement方法产生的标签，
- 可以利用这一特性让这些浏览器支持HTML5新标签，
- 浏览器支持新标签后，还需要添加标签默认的样式：


<br>

参考资料：[w3school](http://www.w3school.com.cn/html/html5_new_elements.asp)









