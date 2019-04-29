---
title: TP 3.2 升级到 PHP 7
date: 2019-04-02 16:35
categories: ['php']
tags: ['php']
comments: true
---

# 题记
TP 3.2 是 PHP 5的版本框架，升级到7的过程中，算是比较平滑，因为之前的代码还算规范，但是也有些坑，这里简单的记录一下，之后应该也没有人会用3.2的框架了吧。

### 坑一：Memcache Session失效
若session采用memcache，则会遇到此问题。
**解决方案：** 修改 ThinkPHP\Library\Think\Session\Driver\memcache.class.php

``` php
    /**
     * 读取Session 
     * @access public 
     * @param string $sessID 
     */
	public function read($sessID) {
        $this->_connect();
        
	    $rs = $this->handle->get($this->sessionName.$sessID);
	    
        $this->_close();
	    
        // 原来
        // return $rs
        return (string)$rs;
	}
```

### END
目前刚开始公司的项目的升级，这篇文章会不定时更新的。