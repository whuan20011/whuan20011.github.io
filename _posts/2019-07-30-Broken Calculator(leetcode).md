---
layout: post
title: Leetcode|Broken Calculator|Python
---

题目大意：有一个坏掉的计算器， 计算器只能进行两种操作， 一种是乘以2， 一种是减1， 现在计算器上显示一个数字X, 问最少经过多少步操作可以使计算器上
数字为Y。

题目详解：这道题是leetcode上面一道medium的题， 但是如果想不出好的解法， 也很难。我假设的表达式为(((X - a)* 2 - b) * 2 - c) * 2 -d ... = Y
我们不知道中间进行了多少步， 所以用...来表示。a, b, c, d等均为自然数， 即可进行多个减1操作， 也可以不进行。但是这样我们也无法解决此题。所以我们想到
a, b, c, d等可以用2n或2n + 1来表示， n为自然数， 如此，（x - a）* 2 - b = （x - a）* 2 - 2n/(2n + 1), 也就等于（x - a - n）* 2 - 0/1.
这样一来， 整个表达式就可以变为(((x - A)* 2 - 0/1) * 2 - 0/1) * 2 - 0/1 .... = Y, 所以X - A = ((（Y + 0 or 1）/ 2 + 0 or 1) / 2 + 0 or 1)
/2 .... 根据Y奇偶性判断加0还是1， 直到等式右边小于等于X结束🔚。

算法：纯数学

```python
class Solution(object):
    def brokenCalc(self, X, Y):
        if X > Y:
            return X - Y
        elif X == Y:
            return 0
        count = 0
        while X < Y:
            if Y % 2 == 0:
                Y = Y / 2
                count += 1
            else:
                Y = (Y + 1) / 2
                count += 2
        count += X - Y
        return count
```
