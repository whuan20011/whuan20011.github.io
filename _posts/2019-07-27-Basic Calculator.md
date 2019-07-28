---
layout: post
title: Leetcode|Basic Calculator|Python
---

题目大意：

题目详解：

算法：字符串

```python
class Solution(object):
    def calculate(self, s):
        arr = []
        tag = 1
        temp = 0
        mark = 1
        dic = {}
        for idx in range(len(s)):
            if s[idx] == "(":
                arr.append(s[idx])
                if tag == -1:
                    dic[len(arr) - 1] = -1
                else:                                                                                                                                                   
                    dic[len(arr) - 1] = 1
                tag = 1
            elif s[idx] == "+":
                if arr and arr[-1] != "(":
                    arr.append(arr.pop() + tag * temp)
                else:
                    arr.append(tag * temp)
                temp = 0
                tag = 1
            elif s[idx] == ")":
                if arr and arr[-1] != "(":
                    arr.append(arr.pop() + tag * temp)
                else:
                    arr.append(tag * temp)
                temp = 0
                cur = arr.pop()
                arr.pop()
                cur *= dic[len(arr)]
                if arr and arr[-1] != "(":
                    arr.append(arr.pop() + cur)
                else:
                    arr.append(cur)
            elif s[idx] == "-":
                if arr and arr[-1] != "(":
                    arr.append(arr.pop() + tag * temp)
                else:
                    arr.append(tag * temp)
                temp = 0
                tag = -1
            elif s[idx].isdigit():
                temp = temp * 10 + int(s[idx])
                if idx == len(s) - 1:
                    arr.append(tag * temp)
            elif s[idx] == " " and s[idx - 1].isdigit():
                if arr and arr[-1] != "(":
                    arr.append(arr.pop() + tag * temp)
                else:
                    arr.append(tag * temp)
                temp = 0
            idx += 1
        return sum(arr)
```
```python
class Solution(object):
    def calculate(self, s):
        s = "(" + s + ")"
        arr = []
        temp = 0
        tag = 0
        for char in s:
            if char.isdigit():
                temp = temp * 10 + int(char)
                tag = 1
            else:
                if char == " ":
                    continue
                if char == "(":
                    arr.append(char)
                if arr and arr[-1] == "(" and tag == 1:
                    arr.append(temp)
                    temp = 0
                    tag = 0
                elif arr and arr[-1] == "+":
                    arr.pop()
                    arr.append(arr.pop() + temp)
                    temp = 0
                    tag = 0
                elif arr and arr[-1] == "-":
                    arr.pop()
                    arr.append(arr.pop() - temp)
                    temp = 0
                    tag = 0
                if char in ["+", "-"]:
                    arr.append(char)
                elif char == ")":
                    cur = arr.pop()
                    arr.pop()
                    if not arr or (arr and arr[-1] == "("):
                        arr.append(cur)
                    if arr and arr[-1] == "+":
                        arr.pop()
                        arr.append(arr.pop() + cur)
                    if arr and arr[-1] == "-":
                        arr.pop()
                        arr.append(arr.pop() - cur)
        return arr[0]
```
