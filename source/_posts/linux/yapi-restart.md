---
title: Yapi启动&重启
date: 2019-07-22 10:37
categories: ['运维']
tags: ['yapi','运维']
comments: true
---

# 题记
记录Yapi常驻进程运行的方法

# 正文
Yapi运行也有一段时间了，由于它的注册是在配置项中进行修改的，所以当修改配置项后，事必要进行 **重启**

之前隐约记得是用 **nohup + &** 就实现了它的常驻进程，**BUT**

没错，***鲁迅说过：“但是”之前的话都是废话（鲁迅：我没说过）***

这次无效了，当关闭了shell窗口，可恶的 502 就出现了。

思考加漫无目的的百度，百度上80%都是我上面的之前的解决方案，另外差不多就是采用node的进程管理方案。

试过 **PM2** 但是不行，执行就报一个 **semver modules** 不存在的错误，但实际上这个modules是存在的

后来采用 **forever** 来处理这个

一次性就好了

```
forever start app.js

forever stop app.js
```
在yapi的 app.js所在目录执行上面的启动命令话，会报

```
-bash: forever: command not found
```
这是命令没加入环境变量导致的，这边我就采用了**绝对路径**的处理方式

找到 nodejs bin 下的 forever

我的路径是在 /usr/local/src/nodejs/bin，那么命令就改成如下

```
/usr/local/src/nodejs/bin/forever start app.js
```

关闭shell窗口，成功！！！！

# 结束语

具体的原因不是太了解，也许node的进程处理是特殊一点的，之后的node相关的应用都会用node的
机制来处理了。
