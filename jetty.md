---
title: 【 jetty 】jetty
date: 2016-01-07 14:38:38
tags:
---

## jetty长时间运行后会出现找不到静态文件情况

首先，在 jetty 目录下运行 `bin/jetty.sh check`，找到 `RUN_CMD = …… -Djava.io.tmpdir=/tmp -jar`。缓存文件保存在 /tmp 目录下，而 linux 一段时间后会删除 tmp文件夹下的文件，导致找不到静态文件，jetty 异常。

解决方法：
修改 jetty.sh，把 tmpdir 指定到系统不会默认清空的文件夹下即可。

`TMPDIR=${TMPDIR:-/jetty/jetty-distribution-8.1.6.v20120903/tmp}`

如果是 `java -jar start.jar` 命令启动 jetty 的话，也可以指定 tmp 目录，
`java -jar start.jar -Djava.io.tmpdir=/tmp`

`kill -9 pid`

`nohup java -jar start.jar >log.out -Djava.io.tmpdir=/home/jetty/jetty-distribution-8.1.21.v20160908/tmp`