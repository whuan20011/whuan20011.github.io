---
layout: post
title: Leetcode|Maximum Subarray Sum with One Deletion|Python
---

题目大意：给一个列表arr， 对于arr的所有非空子列表， 最多可以删掉其中一个元素， 使得子列表的和最大。求其中进行最多删除一个元素的子列表的和最大为多少。

题目限制：

Constraints:

1 <= arr.length <= 10^5

-10^4 <= arr[i] <= 10^4

题目详解：根据题目限制， 我们知道一定不能用O(n^2)的算法， 所以想到如何用O(n)算法来解决， 然而我并没有想出来， 所以本题是我看discuss之后写出来的。
首先我们想到对于任何一个非空字串来说， 我们肯定想删除其中最小负数， 使得字串和最大。如果字串中均为正数，则无需进行删除操作，即和最大。所以如果是O(n)
算法， 碰到负数， 就存在要或不要这个数字的问题。


算法：DP（递推式）

```python
class Solution(object):
    def maximumSum(self, arr):
        """
        :type arr: List[int]
        :rtype: int
        """
        #如果所有数字均为负数， 则输出最大的那个负数。
        count = 0
        for num in arr:
            if num <= 0:
                count += 1
        if count == len(arr):
            return sorted(arr)[-1]
        #ignore是可能会忽略某一个负数产生的当前子列表之和，notignore从来不忽略任何数字， 不管是正数还是负数。他们开始计算之前都设置为0
        ignore = notignore = res = 0
        for num in arr:
            #如果当前数字是自然数，肯定不忽略，所以分别加上这个自然数。如果循环数字一直是自然数，就一直加。
            if num >= 0:
                ignore += num
                notignore += num
            #突然某天来了一个负数， 即当前数字为负数， 我们就想我们要不要忽略掉它，然后继续扫描。所以我们有两种选择。
            #第一种， 我们忽略掉它，即ignore想忽略掉当前数字， 所以之前的它肯定不能忽略（因为最多只能删除一次）， 所以这种情况下ignore的值为notignore。
            #第二种， 我们选择不忽略它（可能前面还有更小的负数， 去删除那个， 所以保留这个， 或者是因为之前已经忽略过了， 所以现在不能忽略了）。
            #所以这种情况下ignore的值为ignore + num
            #那么ignore在这两种情况中取大的。notignore直接加值就可以。
            else:
                ignore = max(notignore, ignore + num)
                notignore += num
            #ignore和notignore均不能为负数， 因为一旦为负数， 就说明它们计算的字串和为负数， 不能向前进行， 只能清零， 不要。
            ignore = max(ignore, 0)
            notignore = max(notignore, 0)
            res = max(res, ignore, notignore)
        return res
```
