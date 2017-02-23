---
title: 【 JS 】事件机制
date: 2016-11-05 16:28:23
tags: JavaScript
---
# 事件流
**事件流描述的是从页面中接收事件的顺序**，IE 的事件流是事件冒泡流，而 Netscape Communicator 的事件流是事件捕获流。

## 事件冒泡
事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。

> 所有现代浏览器都支持事件冒泡，IE5 及更早版本中的时间冒泡会跳过<html> 元素（从 <body> 直接跳到 document）。IE9、Firefox、Chrome 和 Safari 则将事件一直冒泡到 window对象。

```
<div id = 'id2'>
	<div id = 'id2'>
		<div id = 'id3'></div>
	</div>
</div>

var Id1 = document.getElementById('id1');
var id2 = document.getElementById('id2');
var id3 = document.getElementById('id3');

id1.addEventListner('click',function(e){
	console.log("点击了id1");
});
id2.addEventListner('click',function(e){
	console.log("点击了id2");
});
id3.addEventListner('click',function(e){
	console.log("点击了id3");
});
/* 依次点击 id1、id2、id3，输出：id1；id2、id1；id3、id2、id1 */

// 阻止事件冒泡
id2.addEventListner('click',function(e){
	console.log("点击了id2");
	e.stopPropagation();   // 事件到 id2 后不会再向上冒泡
});
/* 依次点击id1、id2、id3，输出：id1；id1、id2；id1、id2、id3 */
```

<br>

## 事件捕获
事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。

> 老版本的浏览器不支持事件捕获，因此建议使用事件冒泡，在有特殊需要时再使用事件捕获。


```
id1.addEventListner('click',function(e){
	console.log("点击了id1");
},true);
id2.addEventListner('click',function(e){
	console.log("点击了id2");
},true);
id3.addEventListner('click',function(e){
	console.log("点击了id3");
},true);
/* 依次点击 id1、id2、id3，输出：id1；id1/id2、id1、id3、id2、id1 */

// 阻止事件捕获
id2.addEventListner('click',function(e){
	e.stopPropagation();  
	console.log("点击了id2");
},true);
/* 依次点击id1、id2、id3，输出：id1；id1、id2；id1、id2 */

```

**事件冒泡与捕获实例**
点击 id3 区域，触发 id2 捕获事件、触发 id3捕获事件、触发 id1冒泡事件，id1、id2、id3均绑定了点击事件处理机制。
```
id1.onclick = function(){    // 事件冒泡
	console.log("点击了id1");
}
id2.addEventListener('click',function(e){  // 事件委托
	console.log("点击了id2");
},true);
id3.addEventListener('click',function(e){   // 事件委托
	console.log("点击了id3");
},true);
/* 依次点击id1、id2、id3，输出id1；id2、id1；id2、id3、id1 */
```

<br>

## 事件委托
对“事件处理程序过多”问题的解决方案就是事件委托。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。
优点：1.提高性能 2.减少代码量

```
<div id="box">
	<div id="id1">
		<div id="id2">
			<div id="id3"></div>
		</div>
	</div>
</div>

window.onload = function(){
	var box = document.getElementById('box');
	box.onclick = function(e){
		var curObj = e.srcElement;
		console.log(curObj.id);
	}
}
```
上述代码在点击 id1、id2、id3、box 时会输出对应元素id。

## DOM事件流
“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的是事件捕获，为截获事件提供了机会；然后是实际的目标接收事件；最后一个阶段是冒泡阶段，可以在这个阶段对事件作出响应。

> IE9、Opera、Firefox、Chrome 和 Safari 都支持 DOM 事件流；IE8 及更早版本不支持 DOM 事件流。

# 事件处理程序
## HTML事件处理程序
`<button onclick='alert("HTML 事件处理程序")'></button>`

有两个缺点：
1. 存在时间差问题，用户可能会在 HTML 元素出现在页面上就触发相应的事件，但当时的事件处理程序不具备执行条件；

2. HTML 与 JavaScript 代码紧密耦合，如果更改事件处理程序，就要同时修改 HTML 代码和 JS 代码。这正是很多开发人员放弃 HTML 事件处理程序，转而使用 **JS 指定事件处理程序** 的原因所在。

## DOM0 级事件处理程序
`btn.onclick = function(){alert("DOM0 级事件处理程序");}`
通过 JS 指定事件处理程序的传统方式，就是 **将一个函数赋值给一个事件处理程序属性**。
优点：简单，有跨浏览器的优势；相比于 HTML 事件处理程序，可以添加多个事件处理程序。


## DOM2级事件处理程序
`btn.addEventListner('click',handler,false);`
定义了两个方法用于处理添加和删除事件处理程序操作：addEventListner() 和 removeEvenetListner()。所有的DOM节点都包含这两个方法，方法接受3个参数：要处理的事件名、事件处理程序的函数和一个布尔值。布尔值参数为true，表示在事件捕获阶段调用事件处理程序；如果为false，表示在冒泡阶段调用事件处理程序。

优点：可以添加多个事件处理程序；

> 大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览器。

## IE事件处理程序
IE实现了与 DOM 类似的两个方法：attachEvent 和 detachEvent。
`btn.attachEvent("onclick",function(){alert("IE事件处理程序")});`
所不同的是，执行两个 onclick 事件处理程序的顺序与定义的顺序相反。

## 跨浏览器（通用）事件处理程序（事件侦听函数）
```
var EventUtil = {
	// 绑定事件
	addHandler : function(element, type, handler){
		if (element.addEventListener) {     // DOM2 级
			element.addEventListener(type, handler, false);
		} else if (element.attachEvent) {   // IE
			element.attachEvent("on" + type, function(){handler.call(element)});
		} else {  
			element["on" + type] = handler; // DOM0 级
		}
	},
	// 移除事件
	removeHandler : function(element, type, handler){
		if (element.removeEventListener) {  // DOM2 级
			element.removeEventListener(type, handler, false);
		} else if (element.detachEvent) {   // IE
			element.detachEvent(type, handler);
		} else {   
			element["on" + type] = null;   // DOM0 级
		}
	}
}
```

# 事件对象
## DOM中的事件对象
兼容 DOM 的浏览器会将一个 event 对象传入到事件处理程序中，无论指定事件处理程序使用的是 DOM0 级方法还是 DOM2 级方法。
```
btn.onclick = function(event){alert(event.type);}  // 'click'
btn.addEventListener('click', function(event){alert(event.type)}, false);   // 'click'
<button onclick='alert(event.type)'></button>
```

事件对象的常用属性：
currentTarget ： 事件处理程序当前正在处理事件的那个元素
target ： 事件的目标
在事件处理程序内部，对象 this 始终等于 currentTarget 的值，而 target 则只包含事件的**实际目标**。
cancelable ：属性为 true 时，才可以使用 preventDefault() 来取消默认行为。

## IE 中的事件对象
在 DOM0 级方法添加事件处理程序时，event 对象作为 window 对象的一个属性存在。
`btn.onclick = function(){alert(window.event.type);}`
如果事件处理程序是使用 attachEvent() 添加的，就会有一个 event 对象作为参数被传入事件处理程序函数中。
`btn.attachEvent('onclick', function(event){alert(event.type)});`

IE 事件对象的常用属性：
srcElement ： 事件的目标（与 DOM 中的 target 属性相同）

## 跨浏览器的事件对象
```
var EventUtil = {
	addHandler : function(element, type, handler){
		// 省略
	},
	removeHandler : function(element, type, handler){
		// 省略
	},
	
	// 返回对 event 对象的引用
	getEvent : function(event){
		return event ? event : window.event;
	},
	// 返回事件的目标
	getTarget : function(event){
		return event.target || event.srcElement;
	},
	// 取消事件的默认行为
	preventDefault : function(event){
		if(event.preventDefault){
			event.preventDefault();
		} else {
			event.returnValue = false;
		}
	},
	// 阻止事件流（IE不支持事件捕获，在 IE 中是阻止事件冒泡）
	stopPropagation : function(event){
		if(event.stopPropagation){
			event.stopPropagation();
		} else {
			event.cancelBubble = true;
		}
	}
}
```

<br>

# 事件类型
要确定浏览器是否支持 DOM2 级事件规定的 HTML 事件(非标准方式支持这些事件的浏览器会返回 fasle)，可以使用：
`var isSupported = document.implementation.hasFeature("HTMLEvents", "2.0")`

要确定浏览器是否支持 DOM3 级事件定义的事件，可以使用：
`var isSupported = document.implementation.hasFeature("UIEvent", "3.0")`

## UI（用户界面事件）
当用户与页面上的元素交互时触发


### load事件
当页面完全加载后（包括所有图像、JS文件、CSS文件等外部资源），就会触发 window 上的load事件。

```
EventUtil.addHandler(window,'load',function(event){alert("Loaded!");});
```
图像上也能触发 load 事件：
```
EventUtil.addHandler(window, 'load', function(){
	var image = document.createElement('img');
	EventUtil.addHandler(image, 'load', function(event){
		event = EventUtil.getEvent(event);
		alert(EventUtil.getTarget(event).src);
	}
});
document.body.appendChild(image);
image.src = 'smile.gif';
```

### unload事件
当文档被完全卸载后触发。用户从一个页面切换到另一个页面，就会发生 unload 事件。这个事件的最常用的情况是：**清除引用，以避免内存泄漏**。

### resize 事件
当浏览器窗口被调整到一个新的高度或宽度时，就会触发 resize 事件。浏览器窗口最大化或最小化时也会触发 resize 事件。

### scroll 事件
在混杂模式下，可以通过 body 元素的 scrollLeft 和 ScrollTop 来监控页面的变化；在标准模式下，除 Safari 之外的所有浏览器都会通过 html 元素来监控这一变化。

## 焦点事件
当元素获得或失去焦点时触发。
**blur**：在元素失去焦点时触发，不会冒泡，所有浏览器都支持；
**focus**：在元素获得焦点时触发，不会冒泡，所有浏览器都支持；
**focusin/focusout**：对应 focus/blur，冒泡，高版本的浏览器支持。

## 鼠标事件 和 滚轮事件
当用户通过鼠标在页面上执行操作时触发
click、dblclick、mousedown、mouseup、mouseenter、mouseleave、mousemove、mouseover、mouseout

### 客户区坐标位置
鼠标指针在**浏览器视口中的位置**。这个位置保存在事件对象的 clientX 和 clientY 属性中，所有浏览器都支持这两个属性。
![客户区坐标](/img/js/event/客户区坐标.png)

### 页面坐标位置
鼠标指针在**页面中的位置**。这个位置保存在事件对象的 pageX 和 pageY 属性中。在页面没有滚动的情况下，pageX 和 pageY 的值与 clientX 和 clientY 的值相等。

IE8 及更高版本不支持事件对象上的页面坐标，不过使用客户区坐标和滚动信息可以计算出来。这时候需要用到 document.body（混杂模式）或 document.documentElement（标准模式）中的 scrollLeft 和 scrollTop 属性。

```
var div = document.getElementById('myDiv');
EventUtil.addHandler(div, 'click', function(event){
	event = EventUtil.getEvent(event);
	var pageX = event.pageX,
	    pageY = event.pageY;

	if(pageX === undefined){
		pageX = event.clientX + (document.body.scrollLeft || document.documentElement.scrollLeft);
	}
	if(pageY === undefined){
		pageY = event.clientY + (document.body.scrollTop || document.documentElement.scrollTop);
	}
	alert("page coordinates: " + pageX + "," + "pageY");
});
```

### 屏幕坐标位置
鼠标指针在**整个电脑屏幕的位置**。用event对象的 screenX 和 screenY 属性表示。
![屏幕坐标位置](/img/js/event/屏幕坐标.png)

## 键盘事件与文本事件
keydown、keypress、keyup、textInput

## HTML5 事件
**contextmenu事件**：单击右键，冒泡；
**beforunload事件**：浏览器在卸载页面之前触发，通过它来区消协在并继续使用原有页面；
**readystatechange事件**：提供文档或元素的加载状态有关的信息，支持该事件的每个对象都有一个 readyState 属性，包含5个值（uninitialized、loading、loaded、interactive、complete）。

<br>

# 问题
## 我们给一个dom同时绑定两个点击事件，一个用捕获，一个用冒泡。会执行几次事件，会先执行冒泡还是捕获？

2次，先执行捕获，再执行冒泡，这是 DOM2 级事件处理程序规定的。

<br>

## 列举IE与其他浏览器不一样的特性？
		
事件不同之处：

触发事件的元素被认为是目标（target）。而在 IE 中，目标包含在 event 对象的 srcElement 属性；

获取字符代码、如果按键代表一个字符（shift、ctrl、alt除外），IE 的 keyCode 会返回字符代码（Unicode），DOM 中按键的代码和字符是分离的，要获取字符代码，需要使用 charCode 属性；

阻止某个事件的默认行为，IE 中阻止某个事件的默认行为，必须将 returnValue 属性设置为 false，Mozilla 中，需要调用 preventDefault() 方法；

停止事件冒泡，IE 中阻止事件进一步冒泡，需要设置 cancelBubble 为 true，Mozzilla 中，需要调用 stopPropagation()；
		
<br>

## 如何使用事件，以及IE和标准DOM事件模型之间存在的差别。
## 事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？

>1. 事件处理机制：IE是事件冒泡、Firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件；
2. ev.stopPropagation();（旧ie的方法 ev.cancelBubble = true;）
3. 在事件中，this 指向触发这个事件的对象，特殊的是，IE 中的 attachEvent 中的 this 总是指向全局对象 Window；


























