---
layout: post
title: Leetcode|Longest Valid Parentheses|python
---

题目大意：给定一个字符串，该字符串只包含"("和")",求该字符串的最大有效长度。

Example 1:
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"

Example 2:
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"

题目详解：该题目比较简单， 解释略。

算法：字符串处理

```python
class Solution(object):
    def longestValidParentheses(self, s):
        if not s:
            return 0
        valid_lens = []
        queue = []
        dic = {}
        for i, char in enumerate(s):
            if char == "(":
                queue.append(i)
            elif char == ")" and queue:
                v = queue.pop()
                dic[i] = v  
        valid_length = []
        for k in dic:
            t = k
            length = 0
            while t in dic:
                length += t - dic[t] + 1
                t = dic[t] - 1
            valid_length.append(length)
        longest = 0
        for v in valid_length:
            longest = max(v, longest)
        return longest      
```
