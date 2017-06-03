---
title: 【 linux 】mysql 安装及配置
date: 2015-10-01 16:17:04
tags: Linux
---

查看操作系统上是否已经安装了 mysql数据库
```
rpm -qa | grep mysql 
```

在 Centos 7 上安装mysql，执行以下语句：
```
wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server
```
或者自行下载 rpm 包进行下载；

需要等待一段时间，如果一直没有下载安装，可能是没有联网，试试 `ping www.baidu.com`，如果不能 ping 成功，则需要查找联网策略。

成功安装后重启mysql服务：
```
service mysqld restart
```

初次安装 mysql 账户为 root，没有密码；Mysql 5.7.6 以后的 5.7 系列版本，使用 `mysqld --initialize-insecure`命令来初始化数据库，不生成随机密码。如果使用默认的命令，则会生成一个随机密码。此时，需要打开 mysql 的配置文件 `/etc/my.cnf`，可以看到 datadir 和 log 文件等的配置信息。打开 `/var/log/mysql.log` 文件，搜做字符串 A temporary password is generated for root@localhost，可以找到自动生成的随机密码。

进入mysql，重新设置初始密码：
```
mysql -uroot
mysql> set password for 'root'@'localhost' = password('mypasswd');
mysql> exit
```

远程授权连接mysql
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

修改编码格式的问题：
在根目录下找到 my.cnf 文件，在文件中加上以下字段：
```
character-set-server=utf8 
collation-server=utf8_general_ci
```
重启生效
停止命令：service mysqld stop
启动命令：service mysqld start

