---
layout: post
title: Leetcode|Decrease Elements To Make Array Zigzag|python
---

题目大意：给一个整数列表， 一次操作指的是对列表中任一元素减1。之字形列表有两种：一种是所有偶数index的元素都比左右两边奇数index元素小， 
另一种是所有奇数index的元素都比左右两边偶数index元素小。问题是， 最少经过多少步减1操作可以将列表变为之字形列表。

题目详解：由于只能进行减1操作， 所以我们只需要考虑波谷即可， 也就是波谷位置的数字一定要比左右两边数字至少小1。这样的话， 我们需要处理奇数index是波谷和偶数
index是波谷两种情况， 同时也要注意边界问题。

算法：列表问题

```python
class Solution(object):
    def movesToMakeZigzag(self, nums):
        count1 = 0
        #偶数index为波谷的情况
        for i in range(len(nums)):
            if i % 2 != 0:
                if i == len(nums) - 1:
                    if nums[i] >= nums[i - 1]:
                        count1 += nums[i] - nums[i - 1] + 1
                else:
                    if nums[i] >= min(nums[i - 1], nums[i + 1]):
                        count1 += nums[i] - min(nums[i - 1], nums[i + 1]) + 1
        count2 = 0
        #奇数index为波谷的情况
        for i in range(len(nums)):
            if i % 2 == 0:
                if i == 0:
                    if nums[i] >= nums[i + 1]:
                        count2 += nums[i] - nums[i + 1] + 1
                elif i == len(nums) - 1:
                    if nums[i] >= nums[i - 1]:
                        count2 += nums[i] - nums[i - 1] + 1
                else:
                    if nums[i] >= min(nums[i - 1], nums[i + 1]):
                        count2 += nums[i] - min(nums[i - 1], nums[i + 1]) + 1
        return min(count1, count2)
```
