---
layout: post
title: Leetcode|Remove Zero Sum Consecutive Nodes from Linked List|Python
---


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeZeroSumSublists(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head:
            return None
        pre = pre_start = ListNode(0)
        pre.next = head
        start = head
        while start:
            sum = 0
            end = start
            while end:
                sum += end.val
                if sum == 0:
                    pre_start.next = end.next
                    break   
                else:
                    end = end.next
            if sum == 0:
                start = pre_start.next
            else:
                start = start.next
                pre_start = pre_start.next
        return pre.next
```
