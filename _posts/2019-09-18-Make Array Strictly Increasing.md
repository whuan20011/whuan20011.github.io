---
layout: post
title: Leetcode|Make Array Strictly Increasing|Python
---

题目大意：给两个整数列表， 返回最少操作次数（可能为0）使得arr1严格递增。一次操作是指用arr2中的某一个数字换掉arr1中的数字， 使arr1严格递增。如果无法使
arr1递增， 返回-1。

题目限制条件：

1 <= arr1.length, arr2.length <= 2000

0 <= arr1[i], arr2[i] <= 10^9

题目详解：本人最近大姨妈造访，停更一星期。现在又继续啦！具体解法如下。（btw, 这道题算法是老公帮我想的， 我自己实现的代码， 只打败了百分之十几的代码，
捂脸！）

算法：DP

```python
import bisect
class Solution(object):
    def makeArrayIncreasing(self, arr1, arr2):
        """
        :type arr1: List[int]
        :type arr2: List[int]
        :rtype: int
        """
        #dic记录arr1中各位置可以取的数字以及如果取该数字， 到这个位置进行了几次操作。所以dic[i] = [[num1, changes1], [num2, changes2],...]
        dic = {}
        #把arr2排序好以便与后面bisect时候用
        arr2 = sorted(arr2)
        #对arr1中的每一个数根据前面dic的记录情况来决定当前位置可选择哪些数字。
        for i in range(len(arr1)):
            dic[i] = []
            #i为0时， 该位置可取本身数字， 也可以替换为arr2中最小的数字。
            if i == 0:
                dic[0].append([arr1[0], 0])
                smallest = arr2[0]
                if smallest < arr1[0]:
                    dic[0].append([smallest, 1])
            else:
                #前一个位置没有取到递增的值， 说明arr1无法严格递增， 返回-1
                if dic[i - 1] == []:
                    return -1
                #arr1最长2000， 最大改变次数随便取了3000
                changes = 3000
                #看前一个位置可以取的数字， 找到当前数字比前一个可取数字大， 但操作次数又最小的那个， 也就是取当前数字的最小次数。若当前数字太小， 
                #也可能没有
                for p in dic[i - 1]:
                    if arr1[i] > p[0] and p[1] < changes:
                        changes = p[1]
                #有的话， 说明该位置可保留当前数字，把它加进字典中 
                if changes != 3000:
                    dic[i].append([arr1[i], changes])
                #再过一遍前一个位置可取数字的情况， 找出在这些数字基础上， arr2中比该数字大的（最小的）那个数字
                for q in dic[i - 1]:
                    bigger_idx = bisect.bisect_right(arr2, q[0])
                    if bigger_idx < len(arr2):
                        #如果位置本身数字没有保留， 只要比之前数字大的都加进字典
                        if changes == 3000:
                            dic[i].append([arr2[bigger_idx], q[1] + 1])
                        #如果位置本身数字可取， 那么就要替换数字比当前位置数字小或者替换次数少（否则换上来也没有优势， 白费！）
                        elif changes < 3000:
                            if sorted(arr2)[bigger_idx] < arr1[i] or q[1] + 1 < changes:   
                                dic[i].append([arr2[bigger_idx], q[1] + 1])
        #找出最后一位最小的替换次数
        least_changes = 3000
        for r in dic[len(arr1) - 1]:
            least_changes = min(r[1], least_changes)
        if least_changes == 3000:
            return -1
        else:
            return least_changes
```
