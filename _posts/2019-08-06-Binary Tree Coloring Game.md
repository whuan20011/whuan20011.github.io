---
layout: post
title: Leetcode|Binary Tree Coloring Game|Python
---

题目大意：给一个二叉树🌳，二叉树中有n个节点(n为奇数)， 有两个游戏玩家， 在第一轮中， 第一个玩家可选任意节点， 涂为红色， 然后第二个玩家也可选任一节点
图为蓝色（不能涂同一节点）。从第二轮开始， 每个玩家都只能涂自己已经涂色的节点的邻居(包括父亲节点， 左右孩子节点)。按照此规则下来，若某一玩家无节点可涂，
就直接给另一个玩家涂色， 如果到最后另一个玩家也没有节点可以涂色， 游戏结束， 看谁的涂色节点多， 谁就是赢家。问题是：给一个数字x， 代表第一个玩家第一次
选择的涂色节点， 我们是第二个玩家， 请问能否有办法保证我们会赢得比赛。

题目详解：因为n为奇数， 所以肯定有赢家， 不会平局。由于题目已经给了第一个玩家第一次选择的节点， 那么就轮到我们去选择节点， 但我们好像并没有什么思路如何
选择节点保证自己赢，继而我想到假设我选完了， 下一轮第一个玩家会选他上次选过的邻居， 也就是父亲节点或左👈节点或右👉节点， 那么我就可以在第一轮中直接选
x的父亲或左右孩子， 要选顺下去涂色邻居最多的那一个。然后看那一个和那一个的邻居们和x所能达到的节点哪个数量多即可。

算法：二叉树🌳

```python
#以下为题目给定二叉树🌳的数据类型
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def btreeGameWinningMove(self, root, n, x):
        #dfs专业去找节点值为x的那个节点
        red = self.dfs(root, x)
        #数出左右节点的个数
        red_left_count = self.count_kids(red.left)
        red_right_count = self.count_kids(red.right) 
        #第一种情况为我第一次选了x左右孩子中邻居多的那个孩子， 第二种为我第一次选了x的父亲， 把其所能达到的邻居和x所能达到的进行比较
        if 2 * max(red_left_count, red_right_count) > n or n > 2 * (red_left_count + red_right_count + 1):
            return True
        return False
    #此函数为找出x节点， 但我开始返回值那里写错了， 多注意返回值写法
    def dfs(self, node, x):
        if not node:
            return None
        if node.val == x:
            return node
        res = self.dfs(node.left, x)
        if res:
            return res
        else:
            return self.dfs(node.right, x)
    #找出节点及其孩子总共的数量
    def count_kids(self, node):
        if not node:
            return 0
        return self.count_kids(node.left) + self.count_kids(node.right) + 1   
        
```
