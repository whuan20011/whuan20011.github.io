---
layout: post
title: Leetcode|Number of Dice Rolls With Target Sum|Python
---

题目大意：有d个骰子🎲， 每个🎲有f个面，每个面标有数字1，2， ...， f。 掷🎲后为使🎲向上的面总和为target， 问有多少种方法。最后返回方法数模
（10^9 + 7）

题目详解：对于这道题， 开始我用了八皇后问题的解题思路， 也就是dfs， 但是超时了。对于八皇后问题， 数量级是八次方， 而本题是f的d次方， f和d均可达30，
所以dfs是不可以的。于是想到dp + 记忆化搜索， 节省了大量时间。核心思想是向前看一步， 递推出现在的情况。也就是前d - 1个骰子的情况推d个骰子的情况。

算法：DP(动态规划 + 记忆化搜索)

```python
class Solution(object):
    def numRollsToTarget(self, d, f, target):
        visited = {}
        return self.dp(d, f, target, visited)
    def dp(self, d, f, target, visited):
        if (d, f, target) in visited:
            return visited[(d, f, target)]
        if target < d or target > d * f or f < 1 or d < 1 or target < 1:
            return 0
        if target == d or target == d * f:
            return 1
        if d == 1 and (target <= f and target > 0):
            return 1
        res = 0
        #递推式
        for i in range(1, f + 1):
            res += self.dp(d - 1, f, target - i, visited)
        visited[(d, f, target)] = res % (1000000000 + 7)
        return res % (1000000000 + 7)       
```
