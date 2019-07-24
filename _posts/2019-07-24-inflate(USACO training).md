---
layout: post
title: USACO training|3.1|Score Inflation|inflate|python
---

题目大意：考试时间是M分钟， 有N种题，给定每一种题的分值和所需要的分钟数， 问在M分钟内， 如何拿到最多的分数。

题目详解：由题目意思可以看出，在有限时间内尽可能得到更多分数， 此为背包👜问题。由于每种题可以做多道， 所以这不是0-1背包👜， 但所有的背包👜问题都是0-1背包👜得来的， 所以此处附加0-1背包👜问题详解。此图由左向右， 由下向上生成。

算法：backpack（背包👜问题）

```python
"""
ID: whuan2001
LANG: PYTHON2
TASK: inflate
"""
def solution():
    fin = open("inflate.in", "r")
    fout = open("inflate.out", "w")
    M, N = map(int, fin.readline().strip().split())
    points_minutes = []
    for line in fin.readlines():
        points_minutes.append(map(int, line.strip().split()))
    #先把类似于上图的表制定好
    chart = []
    for _ in range(N):
        chart.append([0] * (M + 1))
    #本题重点部分，对于每一个时间j来说， 取前i种题目的情况
    for j in range(1, M + 1):
        for i in range(N):
            if i == 0:
                if j >= points_minutes[i][1]:
                    chart[i][j] = (j / points_minutes[i][1]) * points_minutes[i][0]
                else:
                    chart[i][j] = 0
            else:
                if j >= points_minutes[i][1]:
                    chart[i][j] = max(chart[i][j - points_minutes[i][1]] + points_minutes[i][0], chart[i - 1][j])
                else:
                    chart[i][j] = chart[i - 1][j]
    print >>fout, chart[N -1][M]
if __name__ == "__main__":
    solution()    
```
