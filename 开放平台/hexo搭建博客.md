---
title: 【 博客 】hexo 搭建博客
date: 2015-09-05 16:17:04
---

写代码之余，学习了一下用github及hexo搭建博客，主要是总结下自己的学习过程，以前一直用云笔记，但是在共享和可读性方面还是有些欠缺。

废话不多说，这篇文章主要记录下搭博客的流程和可能出现的问题。

<br/>

## 准备工作

### 安装node
命令 `node -v` 查看本机node版本，没有安装则到官网[nodejs](http://nodejs.cn/)下载最新版一路安装即可，附上[百度经验nodejs安装教程](http://jingyan.baidu.com/article/fd8044faf2e8af5030137a64.html)。
<br/>

### 安装git
命令 `git --version` 查看本机git版本，没有安装则到官网[git](https://git-scm.com/downloads)下载一路安装即可，附上[百度经验git安装教程](http://jingyan.baidu.com/article/90895e0fb3495f64ed6b0b50.html)。
<br/>

### 搭建github博客

<br/>
<h4 style='font-size:18px;'>申请github账号并创建仓库</h4>

到[github网站](https://github.com/github)注册一个账号，创建仓库 `username.github.io`，启用GitHub Pages功能。
`username`为你的用户名，比如你的用户名为`matuan`，未来你的网站访问地址就是`https://matuan.github.io`。
<br/>
<h4 style='font-size:18px;'>配置SSH Key</h4>

在用户 `.ssh` 目录下找到 `id_rsa.pub` 文件，如果没有，在该目录下运行命令 `ssh-keygen -t rsa -C "xxx@xxx.com"` 生成，创建过程中按3次回车，密码可以为空。
创建好之后将 `id_rsa.pub` 文件中的密钥复制到 github 用户账户信息 `SSH and GPG keys` 中。　
设置 git 的 user name 和 email：
```
git config --global user.name "xxx"
git config --global user.email "xxx@xxx.com"
```

<br/>
## 安装hexo
### 原理
github pages 存放的是静态文件，但博客不只有文章内容，还有文章列表、分类、标签、翻页等动态内容，如果每次写完一篇文章都要手动更新博文目录和相关链接信息，肯定非常不方便，所以 hexo 所做的就是将这些md文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将改动的页面提交到 github。

### 安装
```
npm install hexo-cli -g
npm install hexo --save
hexo -v
hexo init
npm install
```

需要修改 `_config.xml` 文件中的 `deploy` 配置，将hexo生成的静态页面发布到github上。
<br/>

### 下载主题
我的博客用的是主题是 jacman，命令安装主题，修改`_config.xml`文件中的 theme 配置为 jacman。jacman 内的`_config.xml`文件也可以配置修改。
<br/>

### 使用hexo
在配置中使用 yamllint 来保证自己的 yaml 语法正确。
`hexo n "test"`创建新博文、`hexo s` 启动服务、
`hexo clean` 清除、`hexo g` 生成、`hexo d` 发布
<br>

### 问题处理
```
ERROR Local hexo not found in D:\blog
ERROR Try running:'npm intsall hexo --save'
```

解决方案
```
npm install
hexo server
```
.gitignore 文件里面忽略了node_modules文件夹，所以这个文件夹没有更新上去，用 npm 重新安装即可。
<br/>

### 使用Coding Pages
最初使用 github 的 github Pages 搭建了博客，因为是外网，所以速度比较慢。后使用国内的 [coding](https://coding.net) 管理博客，速度提高了一些，其准备工作以及hexo的安装使用方法是一样的。
在coding上创建名为`yourname.coding.me`的仓库并开启 Pages 服务，在`config.xml`文件中将 deploy 远端地址修改为 coding 上的项目就可以了。
　　
　　
参考资料：[小茗同学的博客园](http://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html) | [最详细的Hexo博客搭建教程](http://sanwen.net/a/bwimzoo.html)