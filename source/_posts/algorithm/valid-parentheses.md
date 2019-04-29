---
title: 有效的括号
date: 2019-02-18 14:56
categories: ['算法']
tags: ['leetcode']
comments: true
---

题目来源：https://leetcode-cn.com/problems/valid-parentheses/

**题目**
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

**注意:** 空字符串可被认为是有效字符串。

**示例**
>输入: "()"
输出: true

>输入: "()[]{}"
输出: true

>输入: "(]"
输出: false

>输入: "([)]"
输出: false

>输入: "{[]}"
输出: true

**分析**
*要想成立，必须要是偶数，另外 2n+1 位是第 n 位 的闭合位置*

**解答**

```PHP
function isValid($s) {
    $string_arr = array(
        '(' => -1,
        ')' => 1,
        '{' => -2,
        '}' => 2,
        '[' => -3,
        ']' => 3,
        '$' => 0
    );
    $length = strlen($s);
    if($length % 2 != 0) return false;
    for($i = 0; $i < $length; $i++){
        if($s[$i] == '$') continue;
        $first = $string_arr[$s[$i]];
        $is_true = false;
        for($j = $i + 1; $j < $length; $j = $j + 2){
            if($first + $string_arr[$s[$j]] == 0){
                $is_true = true;
                $s = substr_replace($s,"$",$j,1);
                $s = substr_replace($s,"$",0,1);
                break;
            }
        }
        if(!$is_true) return false;
    }
    return true;
}
```
**end**
执行效率貌似不高，暂未想到更优解法。

