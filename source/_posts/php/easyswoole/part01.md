---
title: EasySwoole之基础环境搭建
date: 2019-07-27 10:39
categories: ['php']
tags: ['php','easyswoole','swoole']
comments: true
---

# 题记

此篇姑且算是学习swoole的第一篇文章吧。这应该会是一个系列，算是框架的基础上，搭建一套适合自己开发的体系。边学习边捣鼓。

在此之前，看过swoole的官网，github上也下载观看了一些swoole系列的IM,也了解了一下学习swoole的基础知识。但总结下来的感觉，还是需要依托一种框架来实践学习，模糊或有疑问的地方然后再看理论，再实践，才能掌握这个技术点。基于swoole的框架挺多的，而且现在基本上主流的框架也都整合了swoole，也许对于未来的phper，swoole于php犹spring于Java。

扯远了，讲到这篇文章，此文将介绍基础环境搭建，内容的整合来自 EasySwoole的官网内容 [框架安装](https://www.easyswoole.com/Cn/Introduction/install.html) 和 [服务热重载](https://www.easyswoole.com/Cn/Other/hotReload.html)

# 环境

虚拟机：CentOS7

PHP: 7.2.6

Swoole: 4.4.0

EasySwoole: 3.x

# 框架安装

官方说明很详尽 [>>戳我查看<<](https://www.easyswoole.com/Cn/Introduction/install.html)

具体的安装就按照官方一步步来就行了，这里唯一要提一下的就是最后的 **Public** 目录一定要处理

>如果项目还需要使用其他的静态资源文件，建议使用 Nginx / Apache 作为前端Web服务，将请求转发至 easySwoole 进行处理，并添加一个 Public 目录作为Web服务器的根目录
注意!请不要将框架主目录作为web服务器的根目录,否则dev.env,produce.env配置将会是可访问的,也可自行排除该文件(3.1.2已经改为dev.php,produce.php,但依旧建议设置到Public)

这里我用了nginx做反代 官方也说明了 [反向代理](https://www.easyswoole.com/Cn/Introduction/proxy.html)

nginx server的配置如下
```
server {
    root /mnt/hgfs/WorkSpaces/study/easyswoole/Public;
    server_name local.easyswoole.cc;
    location / {
        proxy_http_version 1.1;
        proxy_set_header Connection "keep-alive";
        proxy_set_header X-Real-IP $remote_addr;
        if (!-f $request_filename) {
             proxy_pass http://127.0.0.1:9511;
        }
    }
}

```

这里简单聊一下为何要**设置反代和根目录指向Public**

**反代是为了对外屏蔽端口**

**Public 可以防止配置文件被读取，配置文件包含了很多服务器的一些敏感信息**

算是稍微增加一些服务层面的安全

# 服务热重载

此服务必搭建！！！相信我，不然每次重启，开发时候肯定会崩溃的。并且体验上和原先的开发感一致，改完就可见。

[服务热重载](https://www.easyswoole.com/Cn/Other/hotReload.html)

参考上文处理即可 &#8593;&#8593;&#8593;

# 后记

至此算是搭建好最最最基础的easyswoole的开发环境，当然离相对完整的开发还有

**路由的处理**

**DB**

**......**

未完待续。。。
