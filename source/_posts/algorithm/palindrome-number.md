---
title: 回文数
date: 2019-02-16 15:28
categories: ['算法']
tags: ['leetcode']
comments: true
---
题目来源：https://leetcode-cn.com/problems/palindrome-number/

**题目**
*判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。*

**示例1**
>输入: 121
输出: true

**示例2**
>输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

**示例3**
>输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

**分析**
*从例子中，我们可以看出负数肯定不是回文数，给位数肯定是回文数，只需要判断大于10的数即可，利用字符串来处理是比较简单的。*

**解答**

```PHP
function isPalindrome($x) {
    if($x < 0) return false;
    if($x < 10) return true;
    $string_x = $x . '';
    $length = strlen($string_x);
    $result = true;
    for($i = 0; $i < $length; $i++){
        if($i >= $length - $i - 1) break;
        if($string_x[$i] != $string_x[$length - $i - 1]){
            $result = false;
            break;
        }
    }
    return $result;
}
```
*后来搜了一下PHP的语法，有个* ***strrev函数*** *，用这个方法一句话解决*
```PHP
function isPalindrome($x) {
    return strrev($x) == $x ? true : false;
}
```
*这两个方法在Leetcode提交的上面执行的时间和消耗都差不多*

##进阶版

你能不将整数转为字符串来解决这个问题吗？

**分析**
*这种情况，只能用数学的方法来解决，整除+取余，取余后逆计算，排除中位数的情况，判断一半长度的数据，和官方说的反转一半数字的思路一致*

```PHP
function isPalindrome($x){
    // 排除负数和尾部为0的数据,0 除外，这些肯定不是回文数
    if($x < 0 || ($x % 10 == 0 && $x != 0)) return false;
    $pre = 0;
    while($x > $pre) {
        $pop = $x % 10;
        $x = intval($x/10);
        $pre = $pre * 10 + $pop;
    }
    // 只需要判断相等的情况
    return $x == $pre || $x == intval($pre/10);
}
```
**返回说明**
当数字长度为奇数时，我们可以通过 pre/10 去除处于中位的数字。
例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 $x = 12，$pre = 123，
由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
*(上述文字措辞来自官方，变量修改了一下)*

