---
layout: post
title: Leetcode|Longest Chunked Palindrome Decomposition|Python
---

题目大意：对于字符串来说， 找到它分解之后最大的回文数量。以栗子🌰为进一步解释说明。

Input: text = "ghiabcdefhelloadamhelloabcdefghi"

Output: 7

Explanation: We can split the string on "(ghi)(abcdef)(hello)(adam)(hello)(abcdef)(ghi)".

Input: text = "merchant"

Output: 1

Explanation: We can split the string on "(merchant)".

Input: text = "antaprezatepzapreanta"

Output: 11

Explanation: We can split the string on "(a)(nt)(a)(pre)(za)(tpe)(za)(pre)(a)(nt)(a)".

Input: text = "aaa"

Output: 3

Explanation: We can split the string on "(a)(a)(a)".


题目详解：思路很简单， 用左右指针向中间扫描式寻找。详见代码。

算法：字符串

```python
class Solution(object):
    def longestDecomposition(self, text):
        lens = len(text)
        left = 0
        right = lens - 1
        count = 0
        #temp为每次扫描长度
        temp = 0
        while left < right:
            #当整串字符串左右都没有相等的字串时， 如例2， 完全无回文情况， 扫描到中间即可，节省时间。
            if left + temp > right - temp:
                    count += 1
                    break
            if text[left: left + temp + 1] == text[right - temp: right + 1]:
                #如例3， 扫描到中间只有tpe时候， 虽然left< right, 但强行退出
                if left == right - temp:
                    count += 1
                    break
                #通常情况下👇
                else:
                    count += 2
                    left = left + temp + 1
                    right = right - temp - 1
                    temp = 0
            #找不到相等的字串时
            else:
                temp += 1
        #扫描到中间只剩单独一个字母
        if left == right:
            count += 1
        return count
```
