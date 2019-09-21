---
layout: post
title: Leetcode|K-Concatenation Maximum Sum|Python
---

题目大意：给一个整数序列arr， 把它重复k次， 求其中最大的字串和是多少（子串长度可以为0， 子串和为0）。如果结果太大， 结果模（10^9 + 7）输出。

题目详解：如下。

算法：列表

```python
class Solution(object):
    def kConcatenationMaxSum(self, arr, k):
        """
        :type arr: List[int]
        :type k: int
        :rtype: int
        """
        left_sum = []
        right_sum = []
        l = 0
        r = 0
        #分别求从左边第0位开始到第i位的和和从右边最后一位开始到第len(arr) - i - 1位的和。注：此处不能用sum， 否则变n^2算法。
        for i in range(len(arr)):
            l += arr[i]
            left_sum.append(l)
            r += arr[len(arr) - 1 - i]
            right_sum.append(r)
        a = max(left_sum)
        b = max(right_sum)
        sum_arr = sum(arr)
        #arr的和大于0时，最大子串必为去掉左右，中间k-2个子串和， 再加上从左和从右的最大和。
        if sum_arr > 0:
            if k == 1:
                return ((sum_arr % (1000000000 + 7)) * k) % (1000000000 + 7)
            else:
                return (((sum_arr % (1000000000 + 7)) * (k - 2)) % (1000000000 + 7) + a + b) % (1000000000 + 7)
        #arr和本身小于等于0时
        else:
            #sub先求出arr本身的最大子串和
            sub = self.biggest_subarray(arr)
            if k == 1:
                return sub
            #k > 1时候， 最大子串和为从左最大加从右最大， 然后和sub进行比较
            else:
                return max((a + b), sub) 
    #求出arr本身最大子串和
    def biggest_subarray(self, arr):
        max_sum = 0
        cur_sum = 0
        for i in range(len(arr)):
            cur_sum += arr[i]
            if cur_sum < 0:
                cur_sum = 0
            else:
                if cur_sum > max_sum:
                    max_sum = cur_sum
        return max_sum
```
