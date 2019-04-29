---
title: Windows下Redis服务运行
date: 2019-03-14 10:52
categories: ['Redis']
tags: ['redis','windows','运维']
comments: true
---

# Redis安装

安装并没什么可讲的，我是下载了绿色版的，因此没有自动安装成为windows服务，据说msi版是自动安装成为windows服务的。
具体可参考 [点我，点我，点我](https://github.com/MicrosoftArchive/redis/blob/3.0/Windows%20Service%20Documentation.md)

# 为什么选择服务

窗口，窗口，窗口，一不小心关掉，Redis就挂了。
未安装为服务的时候，Redis服务端的窗口要一直开着，不小心关了，Redis服务也就终止了，所以安装成服务最省心了。强迫症患者必备。

# 安装服务

在进入redis的安装目录后，执行以下的命令

1. 安装服务 
redis-server –service-install redis.windows.conf –loglevel verbose

2. 卸载服务 
redis-server –service-uninstall

3. 启动服务 
redis-server –service-start

4. 停止服务 
redis-server –service-stop

5. 命名服务 
可以启动三个独立的redis服务： 
redis-server –service-install –service-name redisService1 –port 10001 
redis-server –service-start –service-name redisService1

redis-server –service-install –service-name redisService2 –port 10002 
redis-server –service-start –service-name redisService2

redis-server –service-install –service-name redisService3 –port 10003 
redis-server –service-start –service-name redisService3

[原文CSDN](https://blog.csdn.net/sidanchen/article/details/79635715 )
--------------------- 

# END
愉快的在本地开发吧，不用担心窗口被关闭掉，导致Redis没法用