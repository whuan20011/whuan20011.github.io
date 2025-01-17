---
layout: post
title: Leetcode|As Far from Land as Possible|Python
---

题目大意：给一个N * N的网格， 每个网格是0或1， 0代表水，1代表陆地，找到一个水网格使得它到它最近的陆地距离是在所有水网格到最近陆地距离中是最远的。
任意两个网格之间的距离公式为|x0 - x1| + |y0 - y1|。如果没有陆地或者没有水， 返回-1。

题目详解：本题思路比较巧妙。按照正常逻辑， 我们会遍历grid， 对于每一个是水的网格进行bfs向四周扩散， 找到一个陆地立刻返回， 即该水网格到最近陆地的最短
距离。然后把所有的最短距离进行比较找到最长的。但是这样耗时太久， 是无法通过的。

反方向思考😔， 如果我们不从水出发bfs， 如果以陆地为基准bfs向四周（上下左右）扫， 那么先扫到的就是距离陆地的最近距离， 这样讲大大加快效率。 当所有网格
都被扫过， 所有水网格到陆地的最小距离也就都出来了， 从中选取最大的即可。

算法：floodfill（bfs）

```python
class Solution(object):
    def maxDistance(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid[0])
        n = len(grid)
        sum = 0
        queue = []
        for i in range(m):
            for j in range(n):
                sum += grid[i][j]
                #先把所有陆地的网格加到队列里面
                if grid[i][j] == 1:
                    queue.append((i, j, 0))
        #全是陆地或全是水， 返回-1。
        if sum == 0 or sum == m * n:
            return -1
        #建立一个全是0的相同大小网格，反方向思考， 以陆地为基准， 向四周扫， 先扫到的水网格得到的肯定是距离最近陆地的距离。
        #后续更新网格值，每个网格表示自己到最近陆地的距离。
        shortest_dis = [[0] * m] * n
        #此处注意visited要设置为set， list会超时。
        visited = set()
        while queue:
            cur = queue.pop(0)
            if (cur[0], cur[1]) in visited:
                continue
            else:
                visited.add((cur[0], cur[1]))
            #更新网格值
            shortest_dis[cur[0]][cur[1]] = cur[2]
            #继续向该网格四周扫
            if cur[0] - 1 >= 0:
                queue.append((cur[0] - 1, cur[1], cur[2] + 1))
            if cur[0] + 1 < m:
                queue.append((cur[0] + 1, cur[1], cur[2] + 1))
            if cur[1] - 1 >= 0:
                queue.append((cur[0], cur[1] - 1, cur[2] + 1))
            if cur[1] + 1 < n:
                queue.append((cur[0], cur[1] + 1, cur[2] + 1))
        res = 0
        for i in range(len(shortest_dis[0])):
            for j in range(len(shortest_dis)):
                res = max(res, shortest_dis[i][j])
        return res
```

本题再附加一道类似题目, 这道题是我在网上看到其他博主写的， 转载过来， 以便学习。

版权声明：本文为CSDN博主「愚人国王」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/yurenguowang/article/details/77483402

题目大意：

给一个01矩阵，求不同的岛屿的个数。

0代表海，1代表岛，如果两个1相邻，那么这两个1属于同一个岛。我们只考虑上下左右为相邻。

样例 

在矩阵： 
[ 

[1, 1, 0, 0, 0], 

[0, 1, 0, 0, 1], 

[0, 0, 0, 1, 1], 

[0, 0, 0, 0, 0],

[0, 0, 0, 0, 1] 

] 

中有3个岛。


题目详解：深度优先思想。遍历矩阵的每个元素，如果为1则计数加一，同时把自己和周围的元素都置0。

我的想法： 这道题原作者用的dfs， 也可以如上题用bfs来解决(此处不附加bfs解此题代码)。也就是说这道题两种方法都可以。但对于上一道题， 就不能用dfs来解决，
因为深度优先出来的不一定是最近距离， 所以只能广度优先。

算法：dfs

```python
class Solution:
    # @param {boolean[][]} grid a boolean 2D matrix
    # @return {int} an integer
    def numIslands(self, grid):
        # Write your code here
        if grid is None:
            return None
        m = len(grid)
        if m == 0:
            return 0
        n = len(grid[0])
        if n == 0:
            return 0
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    res += 1
                    self.dfs(grid, i, j)
        return res

    def dfs(self, grid, i, j):
        if i < 0 or j < 0 or i >= len(grid) or j >= len(grid[0]):
            return
        if grid[i][j]:
            grid[i][j] = 0
            self.dfs(grid, i + 1, j)
            self.dfs(grid, i - 1, j)
            self.dfs(grid, i, j - 1)
            self.dfs(grid, i, j + 1)
#———————————————— 
#版权声明：本文为CSDN博主「愚人国王」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
#原文链接：https://blog.csdn.net/yurenguowang/article/details/77483402
```
