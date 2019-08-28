---
layout: post
title: Leetcode|Remove Zero Sum Consecutive Nodes from Linked List|Python
---

题目大意：给一个链表， 持续删除其中加和为0的子链表， 最后使得其中没有加和为0的子链表。

题目详解：对于本题， 我先写了解法二， 是常规思路， 是O(n^2)的算法。后面学习了O(n)的算法， 写了解法一。

算法：链表


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None
import collections
class Solution(object):
    def removeZeroSumSublists(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head:
            return None
        #建立一个字典， 使字典的默认值的形式为空列表
        dic = collections.defaultdict(list)
        h = head
        s = 0
        #把到每一个节点之前的链表和作为字典的key，节点作为value
        while h:
            dic[s].append(h)
            s += h.val
            h = h.next
        dic[s].append(h)
        #如果从头到某处有加和为0的一段，h直接跳到某处的后面
        s = 0
        h = dic[0][-1]
        while h:
            #算出到h的加和
            s += h.val
            #找到加和为s的最后一个节点的下一个， 先把h的下一个链接过去，h再跳过去
            h.next = dic[s][-1]
            h = h.next
        return dic[0][-1]
```


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
        #从头开始， 对每一个节点查找其后面的节点， 若出现加和为0的，就删除这段链表
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
