---
title: 重学PHP之属性和方法的重载
date: 2019-04-30 10:56
categories: ['php']
tags: ['php']
comments: true
---

# 题记

之前也了解过这一块的知识，但是没系统看过。这次回顾，系统的记录一下这个使用，加深自己的印象。

ps:在回顾《PHP5 权威指南》中，真的发现自己在写程序中缺少很多设计以及很多特性的使用。

以下内容主要是对书中的内容进行记录。

# 属性和方法的重载

PHP 允许通过实现特殊的代理方法对属性的访问和方法的调用进行重载，这些代理方法将在相关的属性或者方法不存在时调用。

可以实现下面方法的原型

1. function __get($property)
2. function __set($property, $value)
3. function __call($method, $args)

__get 传递属性的名字，并返回属性的值

__set 传递属性的名字和新的值

__call 传递方法的名字和一个数字索引的数组，数组包含传递的参数，第一个参数的索引是0

### __get && __set

```php
class Test{
    private $arr = array(
        'x' => null,
        'y' => null
    );

    function __get($property){
        if(array_key_exists($property, $this->arr)){
            return $this->arr[$property];
        }else{
            print "Error: Can not Read a property other than x and y\n";
        }
    }

    function __set($property, $value){
        if(array_key_exists($property, $this->arr)){
            return $this->arr[$property] = $value;
        }else{
            print "Error: Can not Write a property other than x and y\n";
        }
    }
}

$test = new Test();
$test->x = 1;
print $test->x."\n";

$test->n = 3;
print $test->n;

```

```php
1
Error: Can not Read a property other than x and y
Error: Can not Write a property other than x and y
```

利用上面的方式，很多赋值就不必用那种又臭又长的传参或者统一放在一个看不着的数组里面传入。看着更优雅一点。

### __call

这个使用，就我个人而言，实际可能使用的频率不会向上面的两个那么频繁，框架设计另说了。

```php
class HelloWorld{
    function display($count){
        for($i = 0; $i < $count; $i++){
            print "Hello, World \n";
        }
        return $count;
    }
}

class HelloWorldDelegator{
    private $obj;
    function __construct()
    {
        $this->obj = new HelloWorld();
    }

    function __call($name, $arguments)
    {
        // 书中方法;
        // return call_user_func_array(array($this->obj, $name), $arguments);
        return $this->obj->$name($arguments[0]);
    }
}

$obj = new HelloWorldDelegator();

print $obj->display(3);
```

$arguments 是数组，定义中有说明 **__call 传递方法的名字和一个数字索引的数组，数组包含传递的参数，第一个参数的索引是0**

```php
print $obj->display(3,4);
```

然后打印一下，可以发现

```php
$arguments
array(2) { [0]=> int(3) [1]=> int(4) }
```

call_user_func_array 调用回调函数，并把一个数组参数作为回调函数的参数

[More](https://www.php.net/manual/zh/function.call-user-func-array.php)

这个方法也是很好用的,下面列举具体的使用说明。

```php
function foobar($arg, $arg2) {
    echo __FUNCTION__, " got $arg and $arg2\n";
}
class foo {
    function bar($arg, $arg2) {
        echo __METHOD__, " got $arg and $arg2\n";
    }
}


// Call the foobar() function with 2 arguments
call_user_func_array("foobar", array("one", "two"));

// Call the $foo->bar() method with 2 arguments
$foo = new foo;
call_user_func_array(array($foo, "bar"), array("three", "four"));

```


# 思考

回顾中针对自己也做了一些思考，我目前负责的项目中，许多独立的功能是可以被重写的。

并不是说功能不能用，而是可读性和扩展性上无法和有设计过的相比，这算是是实践后的一种反思吧。

不过实际中的业务场景并不会像demo中的那么简单，**好的设计应该是基于对业务准确的理解上的**。


