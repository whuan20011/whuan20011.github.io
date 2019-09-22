---
layout: post
title: Leetcode|Ugly Number III|Python
---

题目大意：给三个数a, b, c， 找到第n个正整数丑数（丑数是指可被a或b或c整除的正整数）。

题目限制条件：

1 <= n, a, b, c <= 10^9

1 <= a * b * c <= 10^18

It's guaranteed that the result will be in range [1, 2 * 10^9]

题目详解：根据题目限制条件， n都可以达到10^9， 说明O（n）算法肯定也不行。所以， 我们采用数学计算的方式更快。正常我们应该根据第39行得到这个数字， 
但是经过通分， 整除部分可能会丢掉数字， 所以我们通分后得到一个差不多的数字k（32行）， 然后在k左右20000个数字中去找我们真正想要的数字。

算法：纯数学

```python
import math
class Solution(object):
    def nthUglyNumber(self, n, a, b, c):
        """
        :type n: int
        :type a: int
        :type b: int
        :type c: int
        :rtype: int
        """
        k = n * a * b * c // (a * b + a * c + b * c - a - b - c - 2)
        ab = a * b // math.gcd(a, b)
        ac = a * c // math.gcd(a, c)
        bc = b * c // math.gcd(b, c)
        abc = ab * c // math.gcd(ab, c)
        for num in range(max(k - 20000, 1), k + 20000):
            if num % a == 0 or num % b == 0 or num % c == 0:
                if (num // a + num // b + num // c - num // ab - num // ac - num // bc + num // abc) == n:
                    return num
```
