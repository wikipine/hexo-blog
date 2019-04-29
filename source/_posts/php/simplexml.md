---
title: 好用的SimpleXml
date: 2019-04-28 14:56
categories: ['php']
tags: ['php']
comments: true
---

# 题记

此文只是做简单的记录，php中对xml的操作，偶然间看到的，目前实际应用场景中，json格式居多，xml很少，几乎没用过
这篇文章就当是一个概要。之后如果用到，可作为提示之用。

话不多说，直接撸码。

``` php
$xml = "<clients>
<client>
    <name>Boy</name>
    <account>1234567</account>
</client>
<client>
    <name>Girl</name>
    <account>2345678</account>
</client>
</clients>";
$list = simplexml_load_string($xml);
// $file = 'a.xml';
// $file_list = simplexml_load_file($file);
foreach ($list as $v){
    echo $v->name .' account is ' . $v->account.'<br/>';
}
```

**simplexml_load_string** 字面意思，加载string xml

**simplexml_load_file** 字面意思，从文件中加载

加载完成后就能像**对象**那样操作了。

# END

笔记文章，水文一篇