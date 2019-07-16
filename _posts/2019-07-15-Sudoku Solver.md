---
layout: post
title: Leetcode|Sudoku Solver|Python
---

题目大意：给定9\*9棋盘， 其中部分格子为字符型数字1到9中任意一个， 其他格子不是数字的格子为一个点。要求对为点的格子填数字1到9， 使得该格子
所在行和所在列均为数字1到9不重复， 此外该9\*9棋盘被分割为3*3棋盘， 即9块3\*3区域。每个3\*3区域中数字也只能为1到9不重复。最后把该棋盘填满并返回
填写好的棋盘。

题目详解：此题与八皇后（queen8）问题类似，所以也用dfs来解决。首先要检查棋盘中， 如果没有点，即均为数字， 说明棋盘被填写完毕， 可返回棋盘。
如果有点， 则记住第一个为点的格子的坐标， 然后对该格子所在行，列及所在3*3小区域进行检测， 返回该格子可能填写的数字， 对可能填写的数字一个一个进行
递归。由于一个格子可填写数字有多种可能， 所以此处我采用了回溯（backtracking）的方法， 即先改变该格子当前值， 进行递归后再把该格子值改回去。（如果
此处不用回溯， 用copy.deepcopy也可以， 开始我用了此方法， 结果显示超时（Time Limit Exceeded））。

算法：dfs（深度优先算法）

```python
class Solution(object):
    #得到填写好的棋盘， 在原棋盘上进行修改并打印
    def solveSudoku(self, board):
        completed_board = self.dfs(board)
        for row in range(9):
            for col in range(9):
                if board[row][col] == ".":
                    board[row][col] = completed_board[row][col]
        print board
    #检测棋盘
    def check_board(self, board):
        for i in range(9):
            for j in range(9):
                if board[i][j] == ".":
                    return [i, j]
        return False
    #对为点的格子进行递归
    def dfs(self, board):
        check_res = self.check_board(board) 
        if not check_res:
            return board
        i, j = check_res
        #得到该格子可能填写的数字
        temp = self.get_possible_nums(board, i, j)
        #没有数字可填， 说明填写有误， 返回上一层
        if not temp:
            return None
        for num in temp:
            #回溯开始， 先修改此格子的值
            board[i][j] = num
            #得到递归后的返回值， 如果不是None， 即返回了填写完毕的棋盘， 就将这个棋盘向上层函数继续返回， 也就做到了层层返回到最上层
            cur = self.dfs(board)
            if cur != None:
                return cur
            #递归结束， 回溯也要把之前改动的值再修改回来
            board[i][j] = "."
    #得到为点的格子可能填写的数字
    def get_possible_nums(self, board, i, j):
        dic = {}
        for x in range(1, 10):
            dic[str(x)] = False
        for c in range(9):
            if board[i][c] != ".":
                dic[board[i][c]] = True
        for r in range(9):
            if board[r][j] != ".":
                dic[board[r][j]] = True
        for p in range((i / 3) * 3, (i / 3) * 3 + 3):
            for q in range((j / 3) * 3, (j / 3) * 3 + 3):
                if board[p][q] != ".":
                    dic[board[p][q]] = True
        possible_nums = []
        for num in dic:
            if dic[num] == False:
                possible_nums.append(num)
        return possible_nums
```
