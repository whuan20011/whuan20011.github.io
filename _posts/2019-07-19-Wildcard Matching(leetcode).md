---
layout: post
title: Leetcode|Wildcard Watching|Python
---

题目大意：

题目详解：

算法：DP
```python
class Solution(object):
    def isMatch(self, s, p):
        dic = {}
        return self.dp(s, p, len(s), len(p), dic)
    def dp(self, s, p, i, j, dic):
        if (i, j) in dic:
            return dic[(i, j)]
        if i == 0 and j == 0:
            return True
        elif i == 0 and j != 0:
            for k in range(j):
                if p[k] != "*":
                    return False
            return True
        elif j == 0 and i != 0:
            return False
        elif s[i - 1] == p[j - 1] or p[j - 1] == "?":
            dic[(i, j)] = self.dp(s, p, i - 1, j - 1, dic)
            return dic[(i, j)]
        elif p[j - 1] == "*":
            dic[(i, j)] = self.dp(s, p, i, j - 1, dic) or self.dp(s, p, i-1, j, dic)
            return dic[(i, j)]
        else:
            return False
```

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
