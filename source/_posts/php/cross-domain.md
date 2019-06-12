---
title: PHP 之 跨域处理
date: 2019-06-11 14:24
categories: ['php']
tags: ['php','http']
comments: true
---

# 题记

跨域的问题，老生常谈，在前后端分离的项目中非常常见，主要是浏览器的同源策略导致的，解决的方案也非常成熟。这里记录一下自己的
处理心得吧。

### 什么是同源策略？

同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。所以a.com下的js脚本采用ajax读取b.com里面的文件数据是会报错的。

[更多解释](https://www.cnblogs.com/rockmadman/p/6836834.html)

# PHP 中的处理

上面的同源策略中，提到了授权，授权就是同意。Request Header中会携带源的信息，那么我只需要在Response中允许即可，这样子浏览器就
能执行操作了。

PHP中的处理比较简单，以 TP5为例

创建一个中间件

```php
<?php

namespace app\http\middleware;

use think\Response;

class CrossDomain
{
    // 允许访问的 handle
    const ALLOW_HEADERS = [
        'Authorization',
        'Content-Type',
        'If-Match',
        'If-Modified-Since',
        'If-None-Match',
        'If-Unmodified-Since',
        'X-Requested-With',
        'Accept',
        'X-token'
    ];

    // 允许的域名
    const ALLOW_ORIGIN = [
        "http://localhost:8080"
    ];

    public function handle($request, \Closure $next)
    {
        header('Access-Control-Allow-Origin: '.implode(',',self::ALLOW_ORIGIN));
        header('Access-Control-Allow-Headers: '.implode(',',self::ALLOW_HEADERS));
        header('Access-Control-Allow-Methods: GET, POST, PATCH, PUT, DELETE');
        header('Access-Control-Max-Age: 86400');
        if (strtoupper($request->method()) == "OPTIONS") {
            header('status: 204');
            return Response::create()->send();
        }

        return $next($request);
    }
}

```

这样子所有的请求都会被处理到

核心就是 header 里面的 Access-Control-* 部分，基本上就是哪里不允许，就让哪里允许即可。

非中间件的处理，就是在你返回的时候，加上那部分 允许的 header 内容就行了。

细心人的会发现，有个判断

```php
if (strtoupper($request->method()) == "OPTIONS") {
    header('status: 204');
    return Response::create()->send();
}
```

下面就讲一下什么是OPTIONS

# 什么是 OPTIONS？

其实在跨域处理中，这个带OPTIONS的请求不一定会发的，但有时候会必定发送，如上传文件的时候，属于浏览器自动发送的请求。
但之前在写前端的时候，不知道改了什么东西，导致每次发起不同的请求之前，浏览器必定会发起这个请求，但是又不返回内容。这样子可想而知，服务器
每次都要处理一个无意义的请求，这样子绝对是不太好的。

上面说到跨域的授权，那么这个OPTIONS请求，可能就是允许访问的许可证，告诉浏览器，我允许你执行下面这个操作。

[参考 HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

上面的文章，针对这个项目做了非常详细的说明

归纳一下，如何要避免OPTIONS请求，需要满足**简单请求**即可

```php
1.请求方式只能是：GET、POST、HEAD

2.HTTP请求头限制这几种字段（不得人为设置该集合之外的其他首部字段）：
Accept、Accept-Language、Content-Language、Content-Type（需要注意额外的限制）、DPR、Downlink、Save-Data、Viewport-Width、Width

3.Content-type只能取：application/x-www-form-urlencoded、multipart/form-data、text/plain

4.请求中的任意XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问。

5.请求中没有使用 ReadableStream 对象。

```

看了这个之后，再回想自己的项目，发现，多了个 X-token，那么这样我的请求就变成 **非简单请求**了，所以每次都要进行一次预校验处理。

# END

跨域中要想不出现OPTIONS,只做简单请求即可，基本上简单请求能满足大部分的业务需求了，如果不纠结这个，那就允许它在好了。

对了 关于上面的 204 是我之前看到一篇文章里的一句话

> 设置响应状态码为 204 是为了告知客户端表示该响应成功了，但是该响应并没有返回任何响应体，如果状态码为 200，还得携带多余的响应体，在这种场景下是完全多余的，只会浪费流量。

[具体文章->OPTIONS 跨域请求](https://www.cnblogs.com/Robertzewen/p/9959597.html)







