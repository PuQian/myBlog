---
title: 【 Less 】Less预处理器
date: 2016-08-01 22:52:09
tags: CSS
---

## Less 简介
CSS 是一门非程序式语言，没有变量、函数、作用域等概念。Less 在 CSS 的语法基础之上，引入了变量、混入、运算以及函数等功能，简化了CSS的编写，并且降低了CSS的维护成本。
<br/>

## Less 使用方式
### 客户端
从 [Less](http://lesscss.org) 下载 `less.js` 文件，然后在我们需要引入 Less 源文件的 HTML 中加入如下代码：
```
<link rel="stylesheet/less" type="text/css" href="styles.less">
```
Less 源文件的引入方式与标准 CSS 文件引入方式一样。**注意：** Less源文件一定要在less.js之前引入，这样才能保证Less源文件能够正确解析。

### 服务器端
常用的方式是利用 node 的包管理器安装 less，安装成功后就可以在 node 环境中对 less 源文件进行编译。
<br/>

## Less 语法
### 变量
```
@border-color: #222;

.mytheme table{
	border: 1px solid @border-color;
}
```
<br/>

### 作用域
```
@width: 20px;
#home{
	@width: 30px;
	#center{
		width: @width;
	}
}
#leftDiv{
	width: @width;
}
```
<br/>

### 混入
多重继承的一种实现，还有一种混入参数的形式。
```
.roundedCorners(@radius:5px){
	-moz-border-radius: @radius;
	-webkit-border-radius: @radius;
	border-radius: @radius;
}
#header{
	.roundedCorners;
}
#footer{
	.roundedCorners(10px);
}
```
<br/>

### 嵌套
```
#home{
	color: blue;
	#top{
		border: 1px solid #222;
	}
	#center{
		height: 300px;
	}
	#footer{
		height: 300px;
	}
}
```
<br/>

参考资料：[Less官网](http://lesscss.cn/) | [Less框架简介](http://www.ibm.com/developerworks/cn/web/1207_zhaoch_lesscss/)

