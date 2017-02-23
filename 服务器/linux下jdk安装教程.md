---
title: 【 Linux 】 JDK 安装及配置
date: 2015-10-01 16:17:04
tags: Linux
---
## 下载
到 [官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 下载 jdk 软件包  **jdk-8u111-linux-i586.tar.gz**

<br/>

## 解压安装
将安装文件上传到 linux 服务器后，进入到该目录下执行解压安装并删除安装包：
```
cd /home
mkdir java
tar -xvzf jdk-8u111-linux-i586.tar.gz
rm -rf jdk-8u111-linux-i586.tar.gz
```

<br/>

## 配置

### 方式一
编辑 /etc/profile 文件
```
vi /etc/profile
```

添加以下内容：
```
export JAVA_HOME=/home/java/jdk1.8.0_111
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
```

加载刚设置的变量：
```
source /etc/profile
```

<br>

## 方式二
编辑 ~/.bashrc 文件
```
vi /.bashrc
```

<br/>

添加以下内容：
```
export JAVA_HOME=/home/java/jdk1.8.0_111
export JAVA_BIN=$JAVA_HOME/bin
export JAVA_LIB=$JAVA_HOME/lib
export CLASSPATH=.:$JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar
export PATH=$JAVA_BIN:$PATH
```
按ESC键，输入 :wq 保存退出。

<br/>

使 jdk 环境变量生效
```
source ~/.bashrc
```

<br/>

最后查看 jdk 版本
```
java -version
javac -version
```

<br/>

### /etc/profile、~/.bashrc 的区别
/etc/profile 用来设置系统环境参数，**对系统内所有用户有效**；

~/.bashrc 设置系统 bash shell 相关参数，**只针对用户自己而言，不对其他用户生效**。
