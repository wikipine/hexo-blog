---
title: Yapi + Nginx
date: 2019-03-26 16:48
categories: ['Nginx']
tags: ['Nginx','运维']
comments: true
img: http://imgmini.dfshurufa.com/mobile/20160325172758_220bfff17cc1d8fa3fb6c8fae974bde9_2.jpeg
---

# 题记
这篇文章不算从零搭建，只是记录一下其中的坑。

# 环境
客户端：Chrome
服务器：Linux + Nginx

# 正文
Yapi的内网搭建，照官网上来的即可，99.99%不会出问题（0.01%看人品）。

在公司搭建的过程中，运维搭建好，配好域名，在使用过程中发现，在添加，删除，编辑，会出现奇怪的问题，要么JS报错导致页面错误，要么页面没变化，刚开始真的没头绪。

这时候，万能的 **F12** 来了。

看到一个很奇怪的现象

![1_1.png](https://upload-images.jianshu.io/upload_images/1932840-c0791548490a28a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**from disk cache** 字面意思读本地缓存，这就神奇了，这之前没遇到过，点开发现

![1_2.jpg](https://upload-images.jianshu.io/upload_images/1932840-d3e0971331451f18.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

竟然存在**10分钟的缓存**，

比较了我本地搭建的，IP+端口没有缓存的，本地的操作就都是正常的。

至此大胆猜测了一下，就是这缓存导致的，而且假设有缓存的情况，上述的奇怪的现象都能解释得通了。

于是让运维去检查一下nginx里面是不是设置了缓存到期时间。

**果然，然后，就好了。**

# 结束语
其实在之前，运维搭过一次，但是之前在忙别的事，竟然让运维重新搭建，主要自己本地的是好的，然后也是自己不熟nodejs，有点惶恐。

这次问题的解决，发现问题可以慢慢仔细分析，总能解决的。

