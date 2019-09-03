---
layout: post
title: Leetcode|Can Make Palindrome from Substring|Python
---

题目大意：给一个字符串和一个二维列表， 其中每个列表有三个数字， 前两个分别是左右下标， 第三个是k，左右下标表示截取字符串中左到右下标的一段字符串
（包括左右），如果该被截取的字符串能经过不超过k次替换字符（字符串中字符位置可以随意变换， 变换后查看还需要几次替换变回文）就可以变成回文字符串， 
我们就返回true， 否则返回false。

题目限制条件：

1 <= s.length, queries.length <= 10^5

0 <= queries[i][0] <= queries[i][1] < s.length

0 <= queries[i][2] <= s.length

s only contains lowercase English letters.

题目详解：如上题目限制条件， 如果我们对于每一个query截取的一段字符串一个一个去查，将会是10^5 * 10^5次操作， 必定超时。所以我们想了一个简化的办法， 
如下👇（注意第一个注释部分）

算法：字符串

```python
import copy
class Solution(object):
    def canMakePaliQueries(self, s, queries):
        """
        :type s: str
        :type queries: List[List[int]]
        :rtype: List[bool]
        """
        #计算s中到每一个位置， 前面所有出现过的字符个数
        count = []
        for i in range(len(s)):
            if i == 0:
                count.append({s[0] : 1})
            else:
                count.append(copy.copy(count[-1]))
                if s[i] not in count[-1]:
                    count[-1][s[i]] = 1
                else:
                    count[-1][s[i]] += 1
        answer = []
        #对于每一个query， 也就是被截取的每一段， 去count中进行查找， 相减， 模2操作，找到这段字符串中剩下的单个的数量
        for query in queries:
            left = query[0]
            right = query[1]
            times = query[2]
            num_to_change = 0
            for k, v in count[right].items():
                if left == 0:
                    num_to_change += count[right][k] % 2
                else:   
                    if k in count[left - 1]:
                        num_to_change += (count[right][k] - count[left - 1][k]) % 2
                    else:
                        num_to_change += count[right][k] % 2
            #我们只需要替换单个中的一半
            num_to_change /= 2
            if num_to_change <= times:
                answer.append(True)
            else:
                answer.append(False)
        return answer
```
