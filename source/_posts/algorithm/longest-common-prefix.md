---
title: 最长公共前缀
date: 2019-02-16 16:55
categories: ['算法']
tags: ['leetcode']
comments: true
---

题目来源：https://leetcode-cn.com/problems/longest-common-prefix/

**题目**
编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。

**示例**
>输入: ["flower","flow","flight"]
输出: "fl"

>输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。

**分析**
*取出数组的第一个值，然后循环字符串，循环数组，单个判断，一旦不符合，退出循环，返回数据*

**解答**

```PHP
function longestCommonPrefix($strs) {
    $first_arr = $strs[0];
    $str = '';
    for($i = 0; $i < strlen($first_arr); $i++){
        for($j = 1; $j < count($strs); $j++){
            if($strs[$j][$i] != $first_arr[$i]){
                break 2;
            }
        }
        $str .= $first_arr[$i];
    }
    return $str;
}
```
**另外**
官方上有很多其他的算法，上面应该是官方的第一种方案。

