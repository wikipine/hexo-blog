---
title: 两数之和
date: 2019-02-16 09:56
categories: ['算法']
tags: ['leetcode']
comments: true
---

本片是算法学习的第一篇，就多唠嗑几句。

（沉默思考......................）

算了，还是入正题好了

题目来源：https://leetcode-cn.com/problems/two-sum/submissions/

也是朋友安利的一个做题网站【力扣】，个人觉得，闲的时候可以做做题目，保持饥饿感(*^__^*)

其实里面也有官方的题解的，自己写完之后，然后再看一下官方的解答思路，对于小白来说，肯定会有收获的（大神就不要理会了）。

**题目**
*给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。*

**示例**
>给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

```PHP
class Solution {
    function twoSum($nums, $target) {
        $length = count($nums);
        $result = array();
        for($i = 0; $i < $length; $i++){
            for($j = $i + 1; $j < $length; $j++){
                if($nums[$i] + $nums[$j] == $target) {
                    $result = array($i, $j);
                    break 2;
                }
            }
        }
        return $result;
    }
}
```
上面的解答是官方的第一种方法，暴力法，其他的方法，就是空间换时间呗

不过官方说的HASH解法，这个用PHP暂时没实现。

未完待续.............