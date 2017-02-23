---
title: 【 JS 】AJAX 技术
date: 2016-07-20 21:14:20
tags: JavaScript
---

## 简介
AJAX = 异步 Javascript 和 XML，是一种用于创建快速动态网页的技术。

<br/>

## XHR 创建对象
XMLHttpRequest 对象用于在后台与服务器交换数据，这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

```
--- 实例 ---
var xmlhttp;
if(window.XMLHttpRequest){
	xmlhttp = new XMLHttpRequest();
}
else{
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");	
}
```

<br/>

## XHR 请求
### 向服务器发送请求
向服务器发送请求，使用 XMLHttpRequest 对象的 open() 和 send() 方法。
open()：接受三个参数，发送请求名，url 和 是否异步请求的布尔值；
```
xmlhttp.open("GET/POST","xxx.txt",true);
xmlhttp.send();
```
<br/>

### GET 还是 POST
GET 更简单也更快，但是在以下情况下，需要使用 POST 请求：
- 无法使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

<br/>

### GET 请求
```
--- 包含信息：URL、变量值、
xmlhttp.open("GET","/try/ajax/demo_get.php?fname=Henry",true);
xmlhttp.send();

--- 避免得到缓存结果，为每个 URL 添加一个唯一的 ID
xmlhttp.open("GET","/try/ajax/demo_get.php?t=" + Math.random(),true);
xmlhttp.send();
```

<br/>

### POST 请求
```
xmlhttp.open("POST","/try/ajax/demo_post2.php",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Henry&lname=Ford");
```

<br/>

## XHR 响应
要获取来自服务器的响应，使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。

<br/>

### responseText 属性
返回 **字符串形式** 的响应
```
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

<br/>

### responseXML 属性
返回 **XML 形式** 的响应，需要作为 XML 对象进行解析。
```
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i = 0; i< x.length; i++)
{
    txt = txt + x[i].childNodes[0].nodeValue + "<br>";
}
document.getElementById("myDiv").innerHTML=txt;
```

<br/>

## XHR readyState
### onreadystatechange 事件
当请求被发送到服务器时，需要执行一些基于响应的任务，每当 readyState 改变时，就会触发 onreadystatechange 事件，readyState 属性存有 XMLHttpRequest 的状态信息。下面是 XMLHttpRequest 对象的三个重要的属性：
```
1. onreadystatechange：存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。
2. readyState： 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。
		0: 请求未初始化，还没有调用 open 方法
		1: 服务器连接已建立
		2: 请求已接收
		3: 请求处理中
		4: 请求已完成，且响应已就绪
3. status：	200: "OK"、404: 未找到页面
```

实例：
```
xmlhttp.onreadystatechange=function(){
    if (xmlhttp.readyState==4 && xmlhttp.status==200){
    	document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
```
<br/>

### 使用回调函数
回调函数是一种以参数形式传递给另一个函数的函数。
如果您的网站上存在多个 AJAX 任务，那么您应该为创建 XMLHttpRequest 对象编写一个标准的函数，并为每个 AJAX 任务调用该函数。

```
function myFunction()
{
    loadXMLDoc("/try/ajax/ajax_info.txt",function()
    {
        if (xmlhttp.readyState==4 && xmlhttp.status==200)
        {
            document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
        }
    });
}
```

<br/>

## 实例
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script>
function loadXMLDoc()
{
	var xmlhttp;
	if (window.XMLHttpRequest)
	{
		//  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
		xmlhttp=new XMLHttpRequest();
	}
	else
	{
		// IE6, IE5 浏览器执行代码
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange=function()
	{
		if (xmlhttp.readyState==4 && xmlhttp.status==200)
		{
			document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
		}
	}
	xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
	xmlhttp.send();
}
</script>
</head>
<body>

<div id="myDiv"><h2>使用 AJAX 修改该文本内容</h2></div>
<button type="button" onclick="loadXMLDoc()">修改内容</button>

</body>
</html>
```
<br/>

# 问题
## 创建ajax过程
(1)创建XMLHttpRequest对象,也就是创建一个异步调用对象；
(2)创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息；
(3)设置响应HTTP请求状态变化的函数；
(4)发送HTTP请求；
(5)获取异步调用返回的数据；
(6)使用 JavaScript 和 DOM 实现局部刷新。

## 如何解决跨域问题
### JSONP
原理是：动态插入script标签，通过script标签引入一个js文件，这个js文件载入成功后会执行我们在 url 参数中指定的函数，并且会把我们需要的 json 数据作为参数传入。

由于同源策略的限制，XmlHttpRequest 只允许请求当前源（域名、协议、端口）的资源，为了实现跨域请求，可以通过 script 标签实现跨域请求，然后在服务端输出 JSON 数据并执行回调函数，从而解决了跨域的数据请求。

**优点是兼容性好，简单易用，支持浏览器与服务器双向通信。缺点是只支持GET请求。**

JSONP：json+padding（内填充），顾名思义，就是把JSON填充到一个盒子里

```
<script>
    function createJs(sUrl){
 
        var oScript = document.createElement('script');
        oScript.type = 'text/javascript';
        oScript.src = sUrl;
        document.getElementsByTagName('head')[0].appendChild(oScript);
    }
 
    createJs('jsonp.js');
 
    box({
       'name': 'test'
    });
 
    function box(json){
        alert(json.name);
    }
</script>
```

### CORS
服务器端对于 CORS 的支持，主要就是通过设置 Access-Control-Allow-Origin 来进行的。如果浏览器检测到相应的设置，就可以允许 Ajax 进行跨域的访问。

### 通过修改 document.domain 来跨子域
将子域和主域的 document.domain 设为同一个主域。前提条件：这两个域名必须属于同一个基础域名而且所用的协议、端口都要一致，否则无法利用document.domain进行跨域。

主域相同的使用document.domain

### 使用window.name来进行跨域
window对象有个name属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的

### 使用HTML5中新引进的window.postMessage方法来跨域传送数据

还有flash、在服务器上设置代理页面等跨域方式。**个人认为window.name的方法既不复杂，也能兼容到几乎所有浏览器，这真是极好的一种跨域方法。**

## XML和JSON的区别？

(1) 数据体积方面
JSON相对于XML来讲，数据的体积小，传递的速度更快些。
 
(2) 数据交互方面
JSON 与 JavaScript 的交互更加方便，更容易解析处理，更好的数据交互。
 
(3) 数据描述方面
JSON 对数据的描述性比 XML 较差。
 
(4) 传输速度方面
JSON的速度要远远快于XML。

## 说一下什么是javascript的同源策略？

一段脚本只能读取来自于**同一来源的窗口和文档的属性**，这里的同一来源指的是**主机名、协议和端口号的组合**。

## ajax请求时，如何解释json数据

使用eval parse，鉴于安全性考虑 使用parse更靠谱;

## ajax请求的时候get 和post方式的区别?

一个在url后面 一个放在虚拟载体里面
有大小限制
安全问题
应用不同 一个是论坛等只需要请求的，一个是类似修改密码的;

参考资料：[runoob.com](http://www.runoob.com/ajax/ajax-tutorial.html)
