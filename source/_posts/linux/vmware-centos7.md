---
title: VMware + Centos7 安装与配置
date: 2019-05-06 13:39
categories: ['linux']
tags: ['linux','vmware','运维']
comments: true
---

# 题记

生产环境一直是Linux的，但本地的开发环境是Windows的，虽然已经尽可能和线上同步了（如 php的版本一致），但是毕竟底层不同
还是会存在一定上的差别。
一方面也是为了让自己更加熟悉使用Linux的系统，另一方面也是为了之后使用一些新的特性，如swoole。
至于VMware,大学的时候玩过，后来没怎么使用，最近回顾玩一下，并记录一下心得。

# 环境

Win10 专业版 + VMware 15 + CentOS 7

# VMware 安装

VMware的安装就不说明了，密钥网上也一大堆

# CentOS 7

镜像文件可从官网上下载，下面是我使用的链接

[下载链接](http://mirrors.163.com/centos/7.6.1810/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso)

安装的教程，可参考网上的教程，挺多的，12的教程居多，不过基本上一样的。

ps：不建议安装图形界面，好像默认也不是图形界面，这点挺好的。不过如果非要安装，这个要自行查找了，应该很容易就能找到解决方法的。

# 配置

好的，到这一步，默认认为都已经安装好系统了，接下来就介绍一下，个人在配置过程中遇到的问题以及要解决的问题。

Q1:网络配置问题
Q2:CV大法配置,不能复制粘贴，这个怎么能愉快的敲敲敲呢
Q3:共享文件设置，这个很重要，毕竟虚拟机的硬盘不能分配过大，否则本机的空间的不行。另外，修改同一个文件，岂不美哉？！

### **网络配置**

刚配置完系统后，你会发现是并不能上网的，有人可能会说，虚拟机不能上网没什么关系啊。NO,其实很不方便，最直观的例子，yum这个
好东西你就不能用了，安装软件岂不是很痛苦。所以网络的问题要第一解决。

网上一些教程选择桥接模式，这边是默认的NAT模式，在我看来，NAT模式更加适合我对虚拟机的使用，内网模式。

这边采用静态配置，系统默认的是DHCP,动态的，至于静态的理由，很简单，重启的时候不用改IP,毕竟这台虚拟机之后是要作为开发环境使用的，
总不能每次重启，我代码里面还要修改IP吧。

网络的基本属性可在【编辑】->【虚拟网络编辑器】 中查看

![NAT 设置](https://upload-images.jianshu.io/upload_images/1932840-a01b44143752ae2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

NAT设置中的 **网关IP** 在静态配置中会使用到，一般来说不用修改。

打开网卡编辑文件

```php
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

下面是要修改的内容，不存在添加，存在则修改

```php
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.147.128
NETMASK=255.255.255.0
GATEWAY=192.168.147.2
DNS1=114.114.114.114
DNS2=4.4.4.4
```
**ONBOOT** 启动时候加载网卡，上网的关键设置

**DNS** 这个根据自己的网络所在位置来决定，必须配置，否则无法解析域名，限制比较多

修改完后 执行以下命令重启网卡

```php
service network restart
```

就能愉快的ping了

至于本机上虚拟网卡配置，其实不配也不影响使用，因为我上面分配的是100多的，基本上撞的概率很低，如果要配置的，ipv4的静态配置
这里就不介绍了，相信能自行解决的，网关注意一致就行了。

### **CV大法解决**

需要安装 vmware-tools，这个安装比较坑，目测需要理解挂载的机制。不过我安装好后，不知道怎么CV。

这边我的解决方法是 **xshell5 链接到 这台虚拟机上**，利用xshell 来操作，感觉还不赖。

偷鸡界的王者~~~

### **共享文件设置**

这个弄了挺久的，一方面是vm-tools的安装，另一方面好像是挂载的问题。下面会介绍这两个方面。

**安装tools**

在安装系统的时候，注意一下下方，其实会有提示的，这边没有选择安装，而是稍后安装。立即安装应该也只是下载而已，没多大必要。
进入系统后，点击 **【虚拟机】->【重新安装VMwareTools】**,注意下方的弹框，点击 **【帮助】**，不要找网上那些花里胡哨的东西，
点击后跳转的是官方的安装介绍，里面的内容很详细，比网上的一些靠谱多了。

[戳我直接看](https://docs.vmware.com/cn/VMware-Workstation-Pro/15.0/com.vmware.ws.using.doc/GUID-08BB9465-D40A-4E16-9E15-8C016CC8166F.html)

按照官方的一步步下来，看到 enjoy 为止

不过当然不会那么顺理，再执行 vmware-install.pl 的时候可能会有问题的。

注意 **perl**, **gcc** 有无安装，反正我的centos 没有安装的

```php
# 这就用到上面的网络了，不设置无法使用
yum -y install perl

yum -y install gcc
```

完成上面步，基本上可以一路 Enter 下来，不过到 Kernel 的时候，你可以选择 no

然后再一路 Enter 就 ok 了，不过也可以更新 kernel，这样子就能一路 Enter。

完成后 可以看到 **Enjoy** 字样

mnt 目录下会有hfgs的目录

```php
vmware-hgfsclient
```

执行这个命令可以看到你共享的文件夹

共享的设置

![dir_share_set.png](https://upload-images.jianshu.io/upload_images/1932840-c1eef871ea9a4ec6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不过仅执行上一步，还是无法看到共享的文件的

参考：[戳这里](https://blog.51cto.com/853056088/2286704)

这篇文章正解

核心内容

mount -t vmhgfs .host:/py_script /mnt/hgfs

如果出现：Error: cannot mount filesystem: No such device这样的报错

(基本上100%报错，请使用下面的命令)

vmhgfs-fuse .host:/WorkSpaces /mnt/hgfs

### **多文件挂载**

可以建一个空的文件

vmhgfs-fuse .host:/test /mnt/test

host:/test 就是你设置共享时候的名称

/mnt/test 就是系统中的目录位置

### **开机启动**

上述的挂载每次重启后，就要重新执行一遍，所以开机自启挺好的

```php
# 共享的文件全部挂载
echo "vmhgfs-fuse .host:/ /mnt/hgfs" >> /etc/rc.d/rc.local
```
注意一下执行的权限

```php
chmod +x /etc/rc.d/rc.local 
```

之前没有执行的权限，又走了一点弯路

# END

至此基础的系统算是搭建好了，接下就是各个环境的安装，费了不少功夫，不过也算有所得，记录一些，可以防坑。

玩下来的感觉，Linux的入坑之路正式开始。







