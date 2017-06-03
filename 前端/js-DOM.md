---
title: 【 JS 】DOM
date: 2016-05-04 10:27:40
tags: JavaScript
---
# DOM
文档对象模型（Document Object Model）是针对 XML 但经过扩展用于 HTML 的应用程序编程接口。DOM 把整个页面映射为一个多层节点结构。HTML 或 XML 页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。

## DOM级别
DOM1级于1998年成为 W3C 的推荐标准。DOM1级的核心规定是如何映射基于 XML 的文档结构，以便对文档的任意部分进行访问和操作。
DOM2级扩充了鼠标和用户界面事件、范围、遍历等细分模块，而且通过对象接口增加了对 CSS 的支持。
DOM3级也对 DOM 核心进行了扩展，开始支持 XML 1.0规范。
实际上，DOM0级标准是不存在的，所谓的DOM0级不过是历史坐标中的一个参展点，是IE4和Netscape Navigator 4.0最初支持的 DHTML。

## Node 类型
Node 类型是 DOM 1级定义的最基本类型，用于抽象地表示文档中一个独立的部分，除了 IE 以外，在其他浏览器中都可以访问 Node 类型；所有其他类型都继承自 Node。

- **nodeName**　节点的标签名　`节点.nodeName`
- **nodeType**　节点的类型　`节点.nodeType`
- **childNodes**　返回元素所有子节点列表　`Node.childNodes[0]; Node.childNodes.item(0)`
- **firstChild**　返回元素的首个子节点　`Node.firstChild`
- **lastChild**　返回元素的最后一个子节点　`Node.lastChild`
- **parentNode**　返回元素的父节点　`Node.parentNode`
- **previousSibling**　childNodes节点列表中的前一个节点　`Node.previousSibling`
- **nextSibling**　childNodes节点列表中的后一个节点　`Node.nextSibling`
- **appendChild()**　向节点列表的末尾添加一个节点　`父节点.appendChild(newNode) 返回新节点`
- **insertBefore()**　在节点列表的某个特定位置添加节点　`节点.insertBefore(newNode, parent.childNodes[0]) 返回插入节点`
- **replaceChild()**　替换节点　`节点.replaceChild(newNode, parent.firstChild) 返回插入节点`
- **removeChild()**　删除节点　`父节点.removeChild(parent.firstChild) 返回删除节点`
- **cloneNode()**　复制节点　参数 true 为深复制，参数 false 为浅复制，不复制事件处理程序
- **normalize()**　处理文本节点

<br/>

## Document 类型
JS 通过 Document 类型表示文档。在浏览器中，document 对象是 HTMLDocument（继承自 Document 类型）的一个实例，表示整个 HTML 页面，同时 document 对象是 window 对象的一个属性，因此可以将其作为全局对象访问。

- **document.body**　返回 body 元素
- **document.title**　返回文档标题
- **document.URL**　返回 URL
- **document.domin**　返回域名
- **document.referrer**　返回来源页面的 URL
- **getElementById()**　根据ID返回元素
- **getElementsByTagName()**　根据标签名返回元素 `document.getElementsByTagName("img").length`
- **getElementByName()**　根据name特性返回元素，常用于 input 标签

<br/>

## Element类型
Element 类型用于表现 XML 或 HTML 元素，提供了对元素标签名、子节点及特性的访问。

- **getAttribute()**　获取自定义属性，HTML5规范自定义属性应加上data- 前缀以便验证 `Node.getAttribute("data-title")`
- **setAttribute()**　设置自定义属性，不是公认属性，不能使用 node.属性 方法访问　`Node.setAttribute("data-title","value")`
- **attributes**　遍历元素的特性　`Node.attributes["id"].nodeValue`
- **createElement()**　创建新元素 `document.createElement("div")，参数为标签名`

<br/>

## Text类型
文本节点由 Text 类型表示，包含的是可以照字面解释的纯文本内容。

- splitText(offset)  从 offset 开始将当前文本节点分成两个文本节点
- substringData(offset, count) 提取从 offset 开始，长度为 count 的字符串
```
div.firstChild.nodeValue = "Hello World!"  修改文本节点的内容
```
- **createTextNode()**　创建新的文本节点
```
document.createTextNode("Hello World!");
```

<br/>

## Attr 类型
元素的特性在 DOM 中以 Attr 类型来表示，在所有浏览器中，都可以访问 Attr 类型的构造函数和原型。特性就是存在于元素的 attributes 属性中的节点。

## Node 与 Element 的区别
含有完整信息的节点是一个元素，例如一个 `<div></div>`，一个节点不一定是一个元素，但一个元素一定是一个节点。

Node是相当于树这种数据结构而言的，Node有几个子类型：Element、Text、Attribute、Comment、Namespace等。

Element 是 XML 里的概念，Element是可以属性和子节点的Node。

<br/>

# DOM 扩展
虽然 DOM 为与 XML 及 HTML 文档交互制定了一系列核心 API，但仍然有几个规范对标准的 DOM 进行了扩展。这些扩展中有很多是浏览器转悠的，但后来成为了试试标准，于是其他浏览器也都提供了相同的实现。
## 选择符 API
- **querySelector()**　返回根据CSS选择符选择的第一个元素
```
document.querySelector("#parent")
```
- **querySelectorAll()**　返回根据CSS选择符选择的全部元素
```
document.querySelectorAll(".title").length
```

<br/>

## 元素遍历
- **childElementCount**　返回子元素的个数
- **firstElementChild**　返回第一个子元素
- **lastElementChild**　返回最后一个子元素
- **previousElementSibling**　前一个同辈元素
- **nextElementSibling**　后一个同辈元素

<br/>

## HTML5
### 与类相关的扩充
- **getElementByClassName()方法** 根据类名查找元素，支持的浏览器有：IE 9+、Firefox 3+、Safari 3.1+、Chrome 和 Opera 9.5+。
- **classList属性** 
```
add(value)  // 将给定类名添加到列表中，如果值已经存在，则不需要添加
contains(value) // 列表中是否存在给定的值
remove(value)   // 从列表中删除给定的字符串
toggle(value)   // 列表中有值，则删除；列表中没有值，则添加
```

```
div.classList.add("active");
div.classList.remove("active");
div.classList.toggle("active");

// 遍历
for(var i = 0; i < div.classList.length; i++){
	doSomething(div.classList[i]);
}
```

<br/>

### 焦点管理
- **document.activeElement属性**  返回当前获取了焦点的元素
- **document.hasFocus()方法**   返回文档是否获得了焦点

<br/>

### 插入标记
- **innerHTML属性**  读模式下，返回与调用元素的所有子节点对应的 HTML 标记；写模式下，会根据指定的字符串创建新的 DOM 树，然后用这个 DOM 树完全体缓调用元素原先的所有子节点。
- **outerHTLM属性**  读模式下，outerHTML 返回调用其元素及所有子节点的 HTML 标签；写模式下，会根据指定的字符串创建子女的 DOM 子树，然后用这个 DOM 子树完全替换调用元素。

<br>

### jQuery的html()与innerHTML的区别？
jQuery的.html()会调用.innerHTML来操作，但同时也会catch异常，然后用.empty(), .append()来重新操作。 这是因为IE8中有些元素(比如input等)的.innerHTML是只读的。
