---
layout: post
title: Leetcode|Rotate List|Python
---

题目大意：给一个链表， 要求是把这个链表向右转k个位置(k是非负整数)。

题目详解：这道题不难，但是我今年做的第一道链表题目，解析如下。

算法：链表

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        #如果k为0或链表为空都直接返回🔙链表
        if k == 0:
            return head
        if not head:
            return head
        temp = head
        length = 0
        #求出链表长度， 进而得到有效旋转长度
        while temp:
            length += 1
            temp = temp.next
        k %= length
        #有效旋转长度为0，直接返回🔙
        if k == 0:
            return head
        #把链表分成两段， 第一段开始和结尾定义为second和second_end， 然后把second_end找到
        second = head
        second_end = head
        for _ in range(length - k - 1):
            second_end = second_end.next
        #把第二段开头和结尾定义为first和first_end， 把开头和结尾找到， 同时把第一段的结尾指向的下一个置空
        first = second_end.next
        first_end = second_end.next
        second_end.next = None
        while first_end.next:
            first_end = first_end.next
        #把第二段的结尾和第一段的开头连接起来
        first_end.next = head
        return first
```
