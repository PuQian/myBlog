---
title: 【 Webpack 】基础知识
date: 2016-08-19 18:36:04
---
# 模块系统的演进
模块系统主要解决模块的定义、依赖和导出。

## `<script>` 标签
把每个文件看作是一个模块，他们的接口通常是暴露在全局作用域下，定义在 window 对象中，不同模块的接口调用都是在一个作用域中。

原始的加载方式暴露了一些弊端：
> 1. 全局作用域下容易造成变量冲突；
2. 文件只能按照 `<script>` 的书写顺序进行加载；
3. 开发人员必须主观解决模块和代码库的依赖关系；
4. 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪。

## CommonJS
服务器的 Node.js 遵循 CommonJS 规范，该规范的核心思想是允许模块通过 `require` 方法来同步加载所要依赖的其他模块，然后通过 `exports` 或 `module.exports` 来导出需要暴露的接口。

```
require("module");
require("../file.js");

exports.doStuff = function(){};
module.exports = someValue;
```

优点：
> 1. 服务器端模块便于重用；
2. NPM 中已经有将近20万个可以使用的模块包；
3. 简单并容易使用；

缺点：
> 1. 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的；
2. 不能非阻塞的秉性加载多个模块；

## AMD
Asynchronous Module Definition 规范其实只有一个主要接口 `define(id?, dependencires?, factory)`，主要在声明模块的时候指定所有的依赖 `dependencies`，并且还要当做形参传到 `factory` 中，对于依赖的模块提前执行，依赖前置。

```
define("module", ["dep1", "dep2"], function(d1, d2){
	return someExportedValue;
});
require(["module", "../file"], function(module, file){……});
```

优点：
> 1. 适合在浏览器环境中异步加载模块；
2. 可以并行加载多个模块；

缺点：
> 1. 提高了开发版本，代码的阅读和书写比较困难，模块定义方式的语义不顺畅；
2. 不符合通用的模块化思维方式，是一种妥协的实现；

## CMD 


# 渲染的本质
- 前端开发编写维护的 **模板**；
- 运营维护或者后端系统产生的 **数据**；
- 通过一个渲染容器进行合并并产出 **页面**；

# 扩展上下文
- 系统时间、系统环境；
- http 请求上下文；
- 通用工具方法，安全处理包；

# 模板中的静态资源管理
- 前后端统一的资源加载器；
- 基于统一的 seed 控制依赖以及版本；
- 输出 combo url 到页面；
- 依赖自动去重，css/js 头尾加载；

# 模块
拖拽模块来组织页面
```
{{extend("solution://base/")}}
{{#block{"body"}}
	{{use ("mui://headbanner/", $data.modules[1])}}
	{{use ("mui://banner/", $data.modules[2])}}
	{{use ("mui://multi-banner", $data.modules[3])}}
	{{use ("mui://nav", $data.modules[4]) }}
{{/block}}
```
模块化 -- 规模化
- 跨业务共享模块；
- 面向运营的可视化页面搭建平台；
- Native 页面搭建；

渲染服务
- 服务基于 **koa** 框架；
- 扩展 **xtemplate** 模板；
- 阿里云提供 **oss** 提供模板和数据的存储支持；
- 阿里 **CDN** 提供缓存支持；

Express：中间件顺序执行；
Koa：包裹在后面所有装饰器；

# 问题
## 谈谈你对 webpack 的看法

WebPack 是一个模块打包工具，你可以使用 WebPack 管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包 Web开发中所用到的 HTML、Javascript、CSS 以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack 有对应的模块加载器。webpack 模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源。

webpack的两大特色：

1.code splitting（可以自动完成）
 
2.loader 可以处理各种类型的静态文件，并且支持串联操作
webpack 是以commonJS的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。

webpack具有requireJs和browserify的功能，但仍有很多自己的新特性：

1. 对 CommonJS 、 AMD 、ES6的语法做了兼容
2. 对js、css、图片等资源文件都支持打包
3. 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持
4. 有独立的配置文件webpack.config.js
5. 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间
6. 支持 SourceUrls 和 SourceMaps，易于调试
7. 具有强大的 Plugin 接口，大多是内部插件，使用起来比较灵活
8. webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快

参考资料：[Webpack](http://webpackdoc.com/module-system.html)











