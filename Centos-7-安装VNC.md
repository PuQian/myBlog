---
title: 【 Linux 】Centos 7 安装VNC
date: 2015-9-20 23:10:46
tags: Linux
---
VNC 允许 Linux 系统可以类似实现像 Windows 中的远程桌面访问那样访问 Linux 桌面


查看服务器是否安装 VNC
```
# rpm -q tigervnc tigervnc-server
```

没安装的话会出现
```
package tigervnc is not installed
package tigervnc-server is not installed
```

如果没有安装 X-windows 桌面的话要先安装 Xwindows
```
# yum check-update
# yum groupinstall "X Window System"
# yum install gnome-classic-session gnome-terminal nautilus-open-terminal control-center liberation-mono-fonts
# unlink /etc/systemd/system/default.target
# ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target
# reboot
```

第一步，安装 VNC packages:
```
# yum install tigervnc-server -y
```


第二步，修改配置信息，在/etc/systemd/system/下建立文件夹vncserver@:1.service 把example config 文件从/lib/systemd/system/vncserver@.service复制到里面
```
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```


然后打开这个配置文件/etc/systemd/system/vncserver@:1.service替换掉默认用户名
找到这一行
```
ExecStart=/sbin/runuser -l <USER> -c "/usr/bin/vncserver %i"
PIDFile=/home/<USER>/.vnc/%H%i.pid
```


修改登录用户名，如用户名为 root ，修改为
```
ExecStart=/sbin/runuser -l root -c "/usr/bin/vncserver %i"
PIDFile=/root/.vnc/%H%i.pid
```


第三步，重加载 systemd
```
systemctl daemon-reload
```


第四步，为VNC设密码
```
vncpasswd
```


如果报错
```
Job for vncserver@:1.service failed because a configured resource limit was exceeded. 
See "systemctl status vncserver@:1.service" and "journalctl -xe" for details.
```
修改Type 为 `type=simple`


第六步，设默认启动并开启VNC
```
# systemctl enable vncserver@:1.service
# systemctl start vncserver@:1.service
```

`netstat -lp|grep -i vnc` 查看 vnc 在 linux 服务器上的端口号

<br/>

参考资料：[binhu的博客](https://my.oschina.net/huhaoren/blog/497394?p={{totalPage}}) | [yum和apt-get的区别](http://www.centoscn.com/CentOS/Intermediate/2014/0508/2932.html)