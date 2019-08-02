---
layout: post
title: Leetcode|unique letter string|python
---

题目大意：如果一个字符在字符串中只出现过一次， 我们就说这个字符是唯一的。比如字符串“letter”有两个唯一字符， 即“l” 和“r”， 所以"letter"这个字符串的
唯一数为2。问题是给字符串s， 求出s的所有子串的唯一数之和。栗子🌰如下：

Example 1:

Input: "ABC"

Output: 10

Explanation: All possible substrings are: "A","B","C","AB","BC" and "ABC".

Evey substring is composed with only unique letters.

Sum of lengths of all substring is 1 + 1 + 1 + 2 + 2 + 3 = 10

Example 2:

Input: "ABA"

Output: 8

Explanation: The same as example 1, except uni("ABA") = 1.


题目详解：经思考，本题属于dp问题。比如有字符串abcdbab， 假如我们已经知道字符串abcdba的唯一数之和，那么只需要再知道由最后一个b引起的子串的唯一数之和就可以了。
不难发现， 由最后一个b引起子串有b, ab, bab, dbab, cdbab, bcdbab, abcdbab， 但这些子串的唯一数好像比较难求。
我们又发现， 假设把这些子串最后一个b去掉， 子串就是a, ba, dba, cdba, bcdba, abcdba。这刚好就是由倒数第二个a向前引起的子串之和。但由最后一个b引起的
和由最后一个a引起的子串唯一数之和不相等， 那么他们的区别是如果由a引起的子串中没有b， 加最后一个b后该子串唯一数要加1。有一个b， 要减1。有两个b及以上， 不变。
所以我们根据b的idx,就可以得出递推式。

算法：DP（递推）

```python
class Solution(object):
    def uniqueLetterString(self, s):
        #arr中存放s中到当前idx的子串唯一数之和
        arr = []
        #dic中存放s中字符的idx
        dic = {}
        if not s:
            return 0
        if len(s) == 1:
            return 1
        arr.append(1)
        dic[s[0]] = [0]
        if len(s) == 2 and s[0] != s[1]:
            return 4
        if len(s) == 2 and s[0] == s[1]:
            return 2
        if s[0] != s[1]:
            arr.append(4)
            dic[s[1]] = [1]
        else:
            arr.append(2)
            dic[s[0]].append(1)
        #此处为本题重点， 递推式部分
        for i in range(2, len(s)):
            if s[i] not in dic:
                arr.append(arr[i - 1] * 2 - arr[i - 2] + i + 1)
                dic[s[i]] = []
            elif len(dic[s[i]]) == 1:
                arr.append(arr[i - 1] * 2 - arr[i - 2] + i - 2 * dic[s[i]][-1] - 1)
            else:
                arr.append(arr[i - 1] * 2 - arr[i - 2] + i - 2 * dic[s[i]][-1] + 
                           dic[s[i]][-2])
            dic[s[i]].append(i)
        return arr[-1]
```

