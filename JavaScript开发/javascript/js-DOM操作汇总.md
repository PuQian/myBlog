---
title: 【 JS 】DOM 操作汇总
date: 2016-05-04 20:59:04
tags: JavaScript
---
## DOM 创建
var node1 = document.createElement('div');
var node2 = document.createTextNode('hello world!');

<br>

## DOM 查询

// 返回当前文档中第一个类名为 "myclass" 的元素
var el = document.querySelector(".myclass");

// 返回一个文档中所有的 class 为 "note" 或者 "alert" 的 div 元素
var els = document.querySelectorAll("div.note, div.alert");

// 获取元素
var el = document.getElementById('xxx');
var els = document.getElementsByClassName('highlight');
var els = document.getElementsByTagName('td');

var el = ele.firstChild;   //对待标准和非标准模式，如childNods
var el = ele.lastChild;

var el = ele.nextSibling;
var el = ele.previousSibling;

Element也提供了很多相对于元素的DOM导航方法：
// 获取父元素、父节点
var parent = ele.parentElement;
var parent = ele.parentNode;  //只读，没有兼容性问题
  

//获取元素属性列表
var attr = ele.attributes;

// 查询子元素
var els = ele.getElementsByTagName('td');
var els = ele.getElementsByClassName('highlight');

var el = ele.firstElementChild;//非标准不支持
var el = ele.lastElementChild;

var el = ele.nextElementSibling;
var el = ele.previousElementSibling;

// 只读，找到最近的有定位的父节点。
// 没有定位父级时，默认是 body;但在 IE7 以下，如果当前元素没有定位属性，返回body，如果有，返回HTML;
// 如果当前元素某个父级触发了haslayout，则返回触发了haslayout这个元素。
var offsetParent = ele.offsetParent;  

// 获取子节点，子节点可以是任何一种节点，可以通过nodeType来判断
// 标准下、非标准下都只含元素类型，但对待非法嵌套的子节点，处理方式与childNodes一致。  
var nodes = ele.children; 

// 非标准下：只包含元素类型，不会包含非法嵌套的子节点。
// 标准下：包含元素和文本类型，会包含非法嵌套的子节点。 
var nodes = ele.childNodes; 
<br>

## DOM更改
// 添加、删除子元素
ele.appendChild(el);
ele.removeChild(el);

// 替换子元素
ele.replaceChild(el1, el2);

// 插入子元素
parentElement.insertBefore(newElement, referenceElement);

//克隆元素
ele.cloneNode(true) //该参数指示被复制的节点是否包括原节点的所有属性和子节点
<br>

## 属性操作
// 获取一个 {name, value} 的数组
var attrs = el.attributes;

// 获取、设置属性
var c = el.getAttribute('class');
el.setAttribute('class', 'highlight');

// 判断、移除属性
el.hasAttribute('class');
el.removeAttribute('class');

// 是否有属性设置
el.hasAttributes();

<br>

参考资料：[很好玩的博客](http://www.cnblogs.com/shytong/p/4995185.html)