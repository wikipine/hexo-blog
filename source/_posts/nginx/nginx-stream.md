---
title: Nginx之Stream模块使用
date: 2020-06-05 16:48
categories: ['Nginx']
tags: ['Nginx','运维']
comments: true
---

# 题记

使用到这个功能，是因为本机虚拟机的网络环境问题，导致无法访问公司的本地数据库，后面NAT模式解决了，但是在未解决的时候，使用Stream(Nginx反向代理配置)，实现了mysql的端口代理。

# 环境

宿主机：Windows
虚拟机：Linux

# 正文

Stream的模块在0.9版本以上就支持了，不是的话，需要注意一下。

Nginx方面的反代配置是很简单的，主要其实，之后遇到此类问题，可以考虑用Nginx的反代机制来处理。

首页在nginx.conf里面加入stream模块

```
stream {
    include vstream/*.conf;
}
```

在vstream目录里面建一个mysql.conf文件，内容如下

```php
server {
    listen 2333;
    proxy_connect_timeout 10s;
    proxy_pass 192.168.105.200:3306;
}
```

然后重启即可

```
nginx -s reload
```

这样子端口访问 2333 就会转到 200服务器的3306端口了。

# 结束语

Nginx作为反代服务器，真的很有意思，简单的配置就能达到目的，可以好好深入的了解一下它所能提供的功能。

