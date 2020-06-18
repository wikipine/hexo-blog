---
title: Nginx+PHP环境下$_SERVER['PHP_SELF']为空
date: 2020-06-18 14:00
categories: ['php']
tags: ['php','Nginx']
comments: true
---

# 题记

出现此问题是由于老系统使用 TP3.2，本地的开发环境由原先的windows迁移到虚拟机Linux中，然后发现 U 方法后生成的路由会变成相对路由地址，导致重定向的时候会多出一个Controller的名称，这样子加载时候就会路由404。

```php
Example:
正常：/Index/index.html
实际：./Index/index.html
     /Index/Index/index.html
```

# 解决

通过和正常的环境比较，反推到自己本地获取到**$_SERVER['PHP_SELF'] 为空**，实际应该是有值为 index.php

```php
if(!IS_CLI) {
    // 当前文件名
    if(!defined('_PHP_FILE_')) {
        if(IS_CGI) {
            //CGI/FASTCGI模式下
            $_temp  = explode('.php',$_SERVER['PHP_SELF']);
            if (isset($_SERVER['HTTP_HOST'])) {
                define('_PHP_FILE_',    rtrim(str_replace($_SERVER['HTTP_HOST'],'',$_temp[0].'.php'),'/'));
            } else {
                define('_PHP_FILE_',    rtrim($_temp[0].'.php','/'));
            }
        }else {
            define('_PHP_FILE_',    rtrim($_SERVER['SCRIPT_NAME'],'/'));
        }
    }
    if(!defined('__ROOT__')) {
        $_root  =   rtrim(dirname(_PHP_FILE_),'/');
        define('__ROOT__',  (($_root=='/' || $_root=='\\')?'':$_root));
    }
}
```

上面是ThinkPHP.php操作处理的地方。

找到了问题的节点，那么处理起来就比较容易了。查询一下关于 **$_SERVER['PHP_SELF'] **的为空的问题就能解决。

```php
vi /usr/local/php7.1/etc/php.ini
```

修改 cgi_fixpathinfo 改为1，原来我虚拟机里面为0，之后重启 php即可。

```php
/etc/init.d/php-fpm7.1 restart
```

参考： https://www.cnblogs.com/jiqing9006/p/10252087.html

# END

框架过老，且行且珍惜~~~


