---
title: 相同的树
date: 2019-02-22 16:28
categories: ['算法']
tags: ['leetcode']
comments: true
---
题目来源：https://leetcode-cn.com/problems/same-tree/submissions/

**温馨提示**

这篇属于***水文，水文，水文！！！***

**题目**
给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**示例**
```
输入:       1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

输出: true
```
```
输入:      1          1
          /           \
         2             2

        [1,2],     [1,null,2]

输出: false
```
```
输入:       1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

输出: false
```

**分析**
*这个应该是证明 PHP是世界上最好的语言的其中一道算法题了，这道题就当是娱乐吧，底层原理没去深究，只能感叹PHP的数组是真强*

**解答**

```PHP
function isSameTree($p, $q) {
        return $p == $q;
    }
```
**end**
划水片。

