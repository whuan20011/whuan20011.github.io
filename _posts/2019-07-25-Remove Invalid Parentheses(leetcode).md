---
layout: post
title: Leetcode|Remove Invalid Parentheses|Python
---

题目大意：给输入字符串， 里面是左括号，右括号和字母。要求移除最少数量的无效括号，使得输入字符串变得有效。

题目详解：本题我看了hint，如何找出字符串中有几个错误放置的左括号和右括号， 计算方法如下：

0 1 2 3 4 5 6 7

( ) ) ) ( ( ( ) 

i = 0, left = 1, right = 0

i = 1, left = 0, right = 0

i = 2, left = 0, right = 1

i = 3, left = 0, right = 2

i = 4, left = 1, right = 2

i = 5, left = 2, right = 2

i = 6, left = 3, right = 2

i = 7, left = 2, right = 2

找出错误放置的左括号和右括号后， 再对字符串进行深度优先搜索， 得到删除left个左括号和right个右括号后， 就可以找出哪些有效了。本题第二个重点就是
进行dfs时， 使用idx作为参数之一， 每次根据idx， 可以选择删除或不删除当前字符。

算法：

```python
class Solution(object):
    def removeInvalidParentheses(self, s):
        #首先查找出有多少放错位置的左括号和右括号
        left = 0
        right = 0
        for i in range(len(s)):
            if s[i] == "(":
                left += 1
            if s[i] == ")":
                if left >=1:
                    left -= 1
                else:
                    right += 1
        #把当前字符串进行深度优先搜索， 得到删除后所有可能的字符串， 再进行去重
        arr = []
        idx = 0
        self.dfs(s, left, right, arr, idx)
        arr = set(arr)
        #得到有效字符串
        res = []
        for string in arr:
            if self.is_valid(string):
                res.append(string)
        return res
    #深度优先搜索， 对字符串进行删除操作
    def dfs(self, s, left, right, arr, idx):
        if idx == len(s) and (left != 0 or right != 0):
            return
        if left == 0 and right == 0:
            arr.append(s)
            return
        #选择当前删或不删
        if s[idx] == "(" and left > 0:
            self.dfs(s[:idx] + s[(idx + 1):], left - 1, right, arr, idx)
            self.dfs(s, left, right, arr, idx + 1)
        elif s[idx] == ")" and right > 0:
            self.dfs(s[:idx] + s[(idx + 1):], left, right - 1, arr, idx)
            self.dfs(s, left, right, arr, idx + 1)
        #如果当前字符是字母， 则进行下一个
        else:
            self.dfs(s, left, right, arr, idx + 1)
    #判断一个字符串是否有效
    def is_valid(self, s):
        if not s:
            return True
        queue = []
        for char in s:
            if char == "(":
                queue.append(char)
            if char == ")":
                if queue:
                    queue.pop()
                else:
                    return False
        if not queue:
            return True
```
