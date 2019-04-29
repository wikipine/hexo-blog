---
title: 整数反转
date: 2019-02-16 11:57
categories: ['算法']
tags: ['leetcode']
comments: true
---

题目来源：https://leetcode-cn.com/problems/reverse-integer/

**题目**
*给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。*

**示例1**
>输入: 123
输出: 321

**示例2**
>输入: -123
输出: -321

**示例3**
>输入: 120
输出: 21

**注意**
*假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。*

**解答**

```PHP
function reverse($x) {
        $a = $x >= 0 ? 1 : -1;
        $string_num = '' . abs($x);
        $string_new = '';
        for($i = strlen($string_num) - 1; $i >= 0; $i--){
            $string_new .= $string_num[$i];
        }
        // 64位机子和32位，php int值不同，故返回时候多加了个最大值判断
        $new = (int)$string_new;
        $max = pow(2,31);
        if($a == -1){
            if($string_new == $max) {
                $end = $max * -1;
            }else{
                $end = $new != $string_new || $new > $max ? 0 : $new * -1;
            }
        }else{
            $end = $new != $string_new || $new > $max ? 0 : $new;
        }
        return $end;
    }
```

## 官方解法

##### 方法：弹出和推入数字 & 溢出前进行检查

**思路**

我们可以一次构建反转整数的一位数字。在这样做的时候，我们可以预先检查向原整数附加另一位数字是否会导致溢出。

**算法**

反转整数的方法可以与反转字符串进行类比。

我们想重复“弹出” xx 的最后一位数字，并将它“推入”到 \text{rev}rev 的后面。最后，\text{rev}rev 将与 xx 相反。

***要在没有辅助堆栈 / 数组的帮助下 “弹出” 和 “推入” 数字，我们可以使用数学方法。***

>//pop operation:
pop = x % 10;
x /= 10;
//push operation:
temp = rev * 10 + pop;
rev = temp;

但是，这种方法很危险，因为当 \text{temp} = \text{rev} \cdot 10 + \text{pop}temp=rev⋅10+pop 时会导致溢出。

幸运的是，事先检查这个语句是否会导致溢出很容易。

为了便于解释，我们假设rev 是正数。

当rev 为负时可以应用类似的逻辑。

```PHP
function reverse($x) {
        $rev = 0;
        $max = pow(2, 31);
        $min = -$max;
        $max = $max - 1;
        while ($x != 0) {
            $pop = $x % 10;
            $x = intval($x/10);
            if ($rev > $max / 10 || ($rev == $max / 10 && $pop > 7)) return 0;
            if ($rev < $min / 10 || ($rev == $min / 10 && $pop < -8)) return 0;
            $rev = $rev * 10 + $pop;
        }
        return $rev;
    }
```


**心得**
1. 在提交的过程中，校验时候才发了 *64位机子和32位，php int值不同，故返回时候多加了个最大值判断*
算是学到了一点，应该是php版本里面的设置导致的，这点还没深入去了解，另外发现，领扣的校验，这个效率真的不太准，多提交几次，时间还都不一样，发点小牢骚。
2. 纯数学的思路，有时候其实可以省很多代码。

