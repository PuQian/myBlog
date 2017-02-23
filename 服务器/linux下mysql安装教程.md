---
title: 【 linux 】mysql 安装及配置
date: 2015-10-01 16:17:04
tags: Linux
---

查看操作系统上是否已经安装了 mysql数据库
```
rpm -qa | grep mysql 
```

<br/>

在 Centos 7 上安装mysql，执行以下语句：
```
wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server
```

<br/>

需要等待一段时间，成功安装后重启mysql服务
```
service mysqld restart
```

<br/>

初次安装 mysql 账户为 root，没有密码，设置密码：
```
mysql -uroot
mysql> set password for 'root'@'localhost' = password('mypasswd');
mysql> exit
```

<br/>

远程授权连接mysql
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

<br/>