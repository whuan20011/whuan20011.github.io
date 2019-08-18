---
layout: post
title: Leetcode|Maximum Level Sum of a Binary Tree|Python
---

题目大意思：给一颗二叉树， 根节点是第一层， 根节点的孩子是第二层， 以此类推。返回最小的那一层， 使得该层节点总和值最大。

题目详解：对于二叉树的某一层进行计算， 我们知道这一定是bfs。也就是说， 求出每一层节点值之和， 然后找出我们想要的结果。具体分析见代码注释。

算法：bfs + 二叉树

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxLevelSum(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        queue = [root]
        level_sum = []
        while queue:
            temp_sum = 0
            #对每一层进行加和运算， 再将该节点的孩子加入队列。这里要注意虽然queue的长度在加孩子的过程中是变化的， 但len(queue)是在一开始就返回一个
            #当时queue的长度的list， 所以是固定的。
            for _ in range(len(queue)):
                cur = queue.pop(0)
                temp_sum += cur.val
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            level_sum.append(temp_sum)
        max_sum = 0
        for i, sum in enumerate(level_sum):
            if sum > max_sum:
                max_sum = sum
                res = i + 1
        return res
```
