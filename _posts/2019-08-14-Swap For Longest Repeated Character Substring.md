---
layout: post
title: Leetcode|Swap For Longest Repeated Character Substring|Python
---

题目大意：给一个字符串， 我们只可以交换其中两个字符， 问：交换完字符后能形成的连续相同字母的最常字串长度是多少。

题目详解：首先， 我用一个字典先计算一下字符串中各字符的个数，如果字典长度为1， 说明整串字符串中只有一个字符。如果字典长度等于字符串长度， 说明字符串中
各字符均不相同。对于其他情况， 我的想法是假设当前初始字符cur_char是a， 假设后面的字符串为aaabaac， 则计算到b时， 我们把b看作next_char, b的idx
也就是next_idx， 即找完a的情况后， 下一个要从b开始找。当我们发现b之后， 继续向后找， 发现后面仍然是a， 就给next_lens加长度， 表示a隔一个b后的字串长度，
因为可能会有其他地方的a来替换之前的b， 就把由于b隔开的a字串连起来了。当我们扫到c的时候， 我们就知道a所能形成的字串结束了。这时，我们就开始查其他地方
有没有a能来和b交换以便得到a字串的最大长度。

算法：字符串

```python
class Solution(object):
    def maxRepOpt1(self, text):
        dic = {}
        for char in text:
            if char not in dic:
                dic[char] = 0
            dic[char] += 1
        if len(dic) == len(text):
            return 1
        elif len(dic) == 1:
            return len(text)
        cur_char = text[0]
        next_char = ""
        cur_lens = 0
        next_lens = 0
        idx = 0
        start = 0
        longest = 0
        while idx < len(text):
            if text[idx] == cur_char and not next_char:
                cur_lens += 1
                idx += 1
                if idx == len(text):
                    if cur_char in text[:start]:
                        longest = max(longest, cur_lens + next_lens + 1)
                    else:
                        longest = max(longest, cur_lens + next_lens)
            elif text[idx] != cur_char and not next_char:
                next_char = text[idx]
                next_idx = idx
                idx += 1
                if idx == len(text):
                    if cur_char in text[:start]:
                        longest = max(longest, cur_lens + next_lens + 1)
                    else:
                        longest = max(longest, cur_lens + next_lens)
            elif text[idx] == cur_char and next_char:
                next_lens += 1
                idx += 1
                if idx == len(text):
                    if cur_char in text[:start]:
                        longest = max(longest, cur_lens + next_lens + 1)
                    else:
                        longest = max(longest, cur_lens + next_lens)
            elif text[idx] != cur_char and next_char:
                if cur_char in text[idx:] or cur_char in text[:start]:
                    longest = max(longest, cur_lens + next_lens + 1)
                else:
                    longest = max(longest, cur_lens + next_lens)
                cur_char = next_char
                next_char = ""
                cur_lens = 0
                next_lens = 0
                idx = next_idx
                start = next_idx
        return longest
```
