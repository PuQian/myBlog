---
title: 【 JS 】 缓存
date: 2016-02-19 09:50:43
tags: JavaScript
---
# 问题
## 请描述一下 cookies，sessionStorage 和 localStorage 的区别？

cookie 会在浏览器和服务器间来回传递，sessionStorage和localStorage不会；
sessionStorage 和 localStorage的存储空间更大；
sessionStorage 和 localStorage有更多丰富易用的接口；
sessionStorage 和 localStorage各自独立的存储空间；

## 如何实现浏览器内多个标签页之间的通信?

调用localstorge、cookies等本地存储方式

## 如何删除一个cookie

1.将时间设为当前时间往前一点。
```
var date = new Date();
 
date.setDate(date.getDate() - 1);//真正的删除
setDate()方法用于设置一个月的某一天。
```

2.expires的设置
```
  document.cookie = 'user='+ encodeURIComponent('name')  + ';
  expires = ' + new Date(0)
```

## 讲讲304缓存的原理

服务器首先产生ETag，服务器可在稍后使用它来判断页面是否已经被修改。本质上，客户端通过将该记号传回服务器要求服务器验证其（客户端）缓存。

304是HTTP状态码，服务器用来标识这个文件没修改，不返回内容，浏览器在接收到个状态码后，会使用浏览器已缓存的文件

客户端请求一个页面（A）。 服务器返回页面A，并在给A加上一个ETag。 客户端展现该页面，并将页面连同ETag一起缓存。 客户再次请求页面A，并将上次请求时服务器返回的ETag一起传递给服务器。 服务器检查该ETag，并判断出该页面自上次客户端请求之后还未被修改，直接返回响应304（未修改——Not Modified）和一个空的响应体。

