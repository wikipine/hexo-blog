---
title: PHP之Eval函数
date: 2019-04-29 15:54
categories: ['php']
tags: ['php']
comments: true
---

# 题记

回顾翻阅《PHP5 权威指南》这本书的时候，偶尔间看到了 Eval 这个函数。回想起曾经疑惑 W3C 网站或 Leetcode 这些网站上
是怎么**执行用户提交的代码**的，看到它，瞬间开朗了。激动的做了一下小实验。

# eval()

(PHP 4, PHP 5, PHP 7)

eval — 把字符串作为PHP代码执行

**[更多详细官方资料，戳我](https://www.php.net/eval)**

```php
$s = '$var = 222;';
eval($s);
print($var);

```

```php
$func =<<<FUNC
function test(){ 
    echo "test eval function"; 
}
FUNC;
eval($func);
test();
```

***注意***

eval的参数，必须是合法的 PHP 代码。 （ps:在测试中，字符串赋值之类的，老是会忘记封号结尾。)

# END

此函数确实存在如官方所说的风险在，因为允许执行用户输入的代码，所以平时不建议使用。

不过有一类的场景，就是学习类的网站，检验代码执行结果之类的，如 W3C ,Leetcode,ACM之类等等。

用此函数能快速简易的实现代码执行的核心的功能。


