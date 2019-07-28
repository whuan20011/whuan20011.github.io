---
layout: post
title: Leetcode|Basic Calculator|Python
---

题目大意：有一段有效字符串，里面可能含有左括号，右括号，加号➕，减号➖，非负整数，空格。请计算出该字符串的最终结果， 返回🔙一个整数值。

题目详解：本题是一道非常典型的计算器问题。我写了两种方法。方法二比方法一规范一些。

算法：字符串(计算器问题)

方法二：

```python
class Solution(object):
    def calculate(self, s):
        #首先给s左右加括号， 使得arr中总有左括号
        s = "(" + s + ")"
        arr = []
        temp = 0
        tag = 0
        for char in s:
            #isdigit函数可以判断字符是否是数字， 如果是数字， 则对该数字进行计算， 知道某段字符串数字计算完毕
            if char.isdigit():
                temp = temp * 10 + int(char)
                #tag = 1表示我们计算出新的数字了， 避免下面的代码temp = 0时候乱加0
                tag = 1
            #如果该字符不是数字， 如果前面有数字的话，则证明前面的数字计算完毕
            else:
                #这里首先要考虑到空格，直接跳过，不能将计算完毕的数字直接与arr进行处理，假如有段字符串为“5 +”，先处理之前计算的temp和arr,
                #会导致空格和加号时处理了两次temp和arr，所以空格直接先跳过。
                if char == " ":
                    continue
                #左括号的情况也要先放到前面来写， 是因为假如有段字符串为“5+（”， 先处理temp和arr会导致下面到左括号时arr[-1]为加号， 加号会被pop
                #出来，是不对的
                if char == "(":
                    arr.append(char)
                #所以上面在处理了空格和左括号的情况， 我们可以开始处理temp和arr了。
                #第一种arr最后一个为左括号， 并且之前有计算过数字
                if arr and arr[-1] == "(" and tag == 1:
                    arr.append(temp)
                    temp = 0
                    tag = 0
                #第二种arr最后一个为加号， 把temp与之前数字相加
                elif arr and arr[-1] == "+":
                    arr.pop()
                    arr.append(arr.pop() + temp)
                    temp = 0
                    tag = 0
                #第三种同理， 进行相减
                elif arr and arr[-1] == "-":
                    arr.pop()
                    arr.append(arr.pop() - temp)
                    temp = 0
                    tag = 0
                #之前的数字处理结束🔚， 把加号或减号加进去
                if char in ["+", "-"]:
                    arr.append(char)
                #现在字符是右括号时， arr中最后两位必为（cur
                elif char == ")":
                    cur = arr.pop()
                    arr.pop()
                    #以下四种为去掉cur和左括号的情况
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

方法一：

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
