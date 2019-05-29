---
title: PHP之__autoload()
date: 2019-05-29 10:39
categories: ['php']
tags: ['php']
comments: true
---

# 题记

记录一下 __autoload的使用，平时在业务开发中，基本不会使用到，概念是清楚的。Composer,框架基本上都会用到。下面记录一下一个经典的使用案例

# 经典例子

目录结构

![autoload-dir.png](autoload/autoload_dir.png)

MyClass.php

```php
<?php

class MyClass {
    function printHelloWorld(){
        print "hello world";
    }
}
```

general.inc

```php
<?php

function __autoload($classname) {
    require_once './classes/'.$classname.'.php';
}
```

入口文件 main.php

```php
<?php

require_once 'general.inc';

$obj = new MyClass();
$obj->printHelloWorld();

```

输出
> hello world

inc 后缀说明
> inc 文件是include file的意思。实际上，文件的后缀对于文件包含是无所谓,你可以包含一个asp文件，也可以包含txt文。
一般我们使用inc作为后缀，是因为这样能体现该文件的作用。
另外, .inc文件的作用有点类似于C/C++内的.H .HPP头文件，使用inc文件可以使我们的程序，增加可读性


