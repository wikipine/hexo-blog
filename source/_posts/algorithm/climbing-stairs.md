---
title: 爬楼梯
date: 2019-02-22 17:43
categories: ['算法']
tags: ['leetcode']
comments: true
---
题目来源：https://leetcode-cn.com/problems/climbing-stairs/

**题目**
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 n 是一个正整数。

**示例**
```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```
```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

**分析**
其核心算法是***下一个数是前面两个数之和***，前面几个阶梯我们可以很轻松就能知道，2阶2步，3阶3步，那4阶就是5步。

**解答**

```PHP
function climbStairs($n) {
    if($n < 4){
        return $n;
    }
    $num = array(1,2,3);
    $sum = 0;
    for($i = 3; $i < $n; $i++){
        $num[$i] = $num[$i-1] + $num[$i-2];
        $sum = $num[$i];
        unset($num[$i-2]);
    }
    return $sum;
}
```
**end**
空间换时间方法，据说还有一种数学方法，公式不知道，算了。

