---
title: Nginx下Vue的部署
date: 2019-03-18 17:16
categories: ['Nginx']
tags: ['Nginx','vue','运维']
comments: true
---

# 题记
此部署是前后端完全分离，前端域名和后端非同域名的情况，同域名的话，Nginx的配置自行修改，不过在我看来配分个三级域名处理起来更方便直接。

# 环境

Linux + Nginx + Git

# Niginx的配置
Nginx的配置比较简单，也就是入口处的静态资源的加载
```
server {
        listen       80;
        #根目录的位置
        root /home/www/adminwww/iview-admin-dist;
            
        #绑定域名
        server_name   *.test.com;
        location / { 
             index index.html index.htm;
        }
    }

```
# Git
安装git,就是为了拉取代码用，同步更新服务器上的代码，因为每次打包的HASH的关系，文件都是不一样的，因此不能简单的覆盖，**如果当版本更新次数多了的话，传统的上传会导致服务器上无效的文件会增大**，使用Git就能避免此问题。

**本地打包上传到 Master,服务器上 pull 一下，over!!**

Git的安装就不在这里描述，我是用最简单的安装方式
```
yum install git
```
其他安装方式，自行搜索吧

***温馨提示：记得 git pull免密哦***

# END
其实还有更省事的两个方案
**方案一：**Jenkins的持续集成（腾讯云上的那个没搞懂，就放弃了，代码在coding上，本来想这么操作，会爽歪歪的）。
**方案二：**写个shell脚本，后台写个方法，点击触发git pull事件，这样子就可以不用登服务器了。

本着负责任的态度，截至2019年3月18号，我没实现。以后实现了，再加吧。

拜拜，谢谢观看。


