---
layout: post
title: Leetcode|Wildcard Watching|regular expression|Python
---

说明：今天我会在一篇博客中写两道题wildcard watching和regular expression。因为这两道题题目和解法非常相似。第二道题的解法也是学习了第一道题而得来的。

题目(wildcard watching)大意：有两个字符串s和p。s字符串可以为空串或者只包含a到z中小写字母。p字符串可以为空串，也可以有任何小写字母，也可以含有“？”或者“*”。其中问号可以和任何一个小写字母匹配， 星号可以表示空字符串或者任何其他字符串。问题是：给两个字符串s和p， 判断p能否匹配s。

题目详解：这道题我开始自己写了解法二， 最终调试成功， 但是超时了。所以又学习了解法一的动态规划法。此处解法二只作为参考，只解释动态规划的方法。
动态规划+记忆化搜索是非常常见的， 本题一共分为六种情况进行讨论， 其中第五种是本题关键。具体解释如下注释。


解法一（算法：DP）：
```python
class Solution(object):
    def isMatch(self, s, p):
        #为避免超时， 用dic做了记忆化搜索
        dic = {}
        return self.dp(s, p, len(s), len(p), dic)
    #动态规划法进行检测
    def dp(self, s, p, i, j, dic):
        #首先检查是否已经搜索过
        if (i, j) in dic:
            return dic[(i, j)]
        #此处开始进行分类讨论， 第一种为如果已经检索到尽头， 即均为空串，匹配成功。
        if i == 0 and j == 0:
            return True
        #第二种s检索到头， p还有， 那么p只可以为一串星号， 不能有其他字母。
        elif i == 0 and j != 0:
            for k in range(j):
                if p[k] != "*":
                    return False
            return True
        #第三种p检索到尽头， s还有， 匹配不成功。
        elif j == 0 and i != 0:
            return False
        #第四种s和p均未到尽头， 即检索最右一个字母， 如果相同或p最后一个字母为问好， 最后一个匹配成功，继续向前匹配。
        elif s[i - 1] == p[j - 1] or p[j - 1] == "?":
            dic[(i, j)] = self.dp(s, p, i - 1, j - 1, dic)
            return dic[(i, j)]
        #第五种也是s和p均为到尽头， 但p的最后一个字母为星号，一种情况为星号表示空串，另一种情况为星号把当前字母匹配掉， 但星号保留， 即j不变， 
        #i变为i - 1
        elif p[j - 1] == "*":
            dic[(i, j)] = self.dp(s, p, i, j - 1, dic) or self.dp(s, p, i-1, j, dic)
            return dic[(i, j)]
        #其他情况均为不匹配。
        else:
            return False
```

解法二（分类法）
```python
class Solution(object):
    def isMatch(self, s, p):
        dic = {}
        if self.is_match(s, p, dic):
            return True
        else:
             False
    def is_match(self, s, p, dic):
        if (s, p) in dic:
            return dic[(s, p)]
        if s == p:
            dic[(s, p)] = True
            return True
        if p == "*":
            return True
        if len(s) == 1 and len(p) == 1:
            if s != p and p == "?":
                return True
            if s != p and p != "?":
                return False
        if len(s) == 1 and len(p) > 1:
            count = 0
            for char in p:
                if char == "*":
                    count += 1
                else:
                    mark = char
            if count == len(p):
                return True
            elif count == len(p) - 1 and (mark == s or mark == "?"):
                return True
            else:
                return False
        elif len(s) > 1 and len(p) > 1:
            if s[0] == p[0] or (s[0] != p[0] and p[0] == "?"):
                if self.is_match(s[1:], p[1:], dic):
                    dic[(s[1:], p[1:])] = True
                    return True
                else:
                    dic[(s[1:], p[1:])] = False
                    return False
            if s[0] != p[0] and p[0] != "?" and p[0] != "*":
                return False
            if p[0] == "*":
                tag = 0
                for k in range(1, len(p)):
                    if p[k] != "*":
                        tag = 1
                        break
                if tag == 0:
                    return True
                for i in range(len(s)):
                    temp = 0
                    if p[k] == "?" or s[i] == p[k]:
                        if self.is_match(s[i:], p[k:], dic):
                            temp = 1
                            dic[(s[i:], p[k:])] = True
                            return True
                if temp == 0:
                    dic[(s[i:], p[k:])] = False
                    return False
```
