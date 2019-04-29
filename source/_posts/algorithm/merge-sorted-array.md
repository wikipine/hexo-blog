---
title: 合并两个有序数组
date: 2019-02-18 18:25
categories: ['算法']
tags: ['leetcode']
comments: true
---

题目来源：https://leetcode-cn.com/problems/merge-sorted-array/

**题目**
给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。

说明:

- 初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
- 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。

**示例**
>输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
输出: [1,2,2,3,5,6]

**分析**
*此题应该主要考察数组合并以及排序，不过使用 PHP ,并未去实现底层，直接用自带的方法了，就当是一个记录吧*

**解答**

```PHP
function merge(&$nums1, $m, $nums2, $n) {
    $nums1 = array_slice($nums1, 0, $m);
    $nums2 = array_slice($nums2, 0, $n);
    $nums1 = array_merge($nums1 , $nums2);
    sort($nums1);
    return $nums1;
}
```
**end**
主要用到 ***array_slice*** , ***array_merge*** 以及最后的排序 ***sort*** ,偷了一把鸡。

