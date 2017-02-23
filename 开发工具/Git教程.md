---
title: 【 Git 】Git教程
date: 2016-01-12 22:42:53
categories: git
tags: Git
---

从前实验室使用 svn 进行版本控制，逐渐被种草 git，在实验室服务器上搭了 gitlab，用起来还是不错的，总结一下git的常用命令，具体的教程直接看廖雪峰老师的官方网站就好了，讲得很好。
<br/>

### 创建版本库
```
pwd                        显示当前目录
git init                   初始化仓库
git add <file>             将工作区的文件添加到暂存区
git add .                  将工作区所有修改文件添加到暂存区
git commit -m "comment"    将暂存区的内容提交仓库
git status                 查看工作区和暂存区的状态
git diff <file>            查看file具体修改的内容
```
<br/>

### 连接远程仓库
```
git remote                 查看远程库的信息
git remote -v              查看远程库的详细信息
git config --global user.name "浦倩"                  添加全局用户名
git config --global user.email "puqian1992@163.com"   添加全局邮箱
git push origin master     推送至master分支
git clone git@git-KVM:puqian/wms.git  拷贝远端内容至本地
```
<br/>

### 版本回退
```
git log       查看提交历史
git log --pretty=online   显示简洁的历史记录
git relog     查看命令历史

git reset --hard HEAD^    暂存区回退到上一个版本
git reset --hard HEAD^^   暂存区会退到上两个版本
```
<br/>

### 撤销修改
```
git reset HEAD file   撤销暂存区文件内容的修改
git checkout --file   撤销工作区修改内容
```
<br/>

### 删除文件
```
rm file       从文件管理器中删除文件
git rm file   从版本库中删除文件
```
<br/>

### 分支管理
```
git checkout -b dev     创建并切换到dev分支（相当于 git branch dev; git checkout dev;）
git branch              查看当前所有分支
git checkout master     切换到master分支
git merge dev           将dev分支的工作成果合并到master分支上
git branch -d dev       删除dev分支
```
<br/>

### 标签
```
git tag                      查看所有标签信息
git tag <name>               打开一个新标签
git push origin <tagname>    推送一个本地标签
git push origin --tags       推送全部未推送过的本地标签
```
<br/>

### 实例
```
--- Git global setup ---
git config --global user.name  "浦倩"
git config --global user.email "xxx@xxx"

--- Create a new repository ---
git clone git@git-KVM:puqian/wms.git
cd wms
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

--- Existing  folder or Git repository ---
cd existing_folder
git init
git remote add origin git@git-KVM:puqian/wms.git
git add .
git commit -m "xxx"
git push -u origin master
```
<br/>

# 问题
## git fetch和git pull的区别
git pull：相当于是从远程获取最新版本并merge到本地
git fetch：相当于是从远程获取最新版本到本地，不会自动merge


参考资料：[廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)