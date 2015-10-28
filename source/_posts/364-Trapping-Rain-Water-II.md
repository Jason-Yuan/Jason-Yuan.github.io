title: "364 Trapping Rain Water II"
date: 2015-10-07 02:29:19
tags: ["LintCode", "Array", "BFS", "Priority Queue"]
categories: "LintCode"
---

# 原题
>给出 n * m 个非负整数，代表一张X轴上每个区域为 1 \* 1 的 2D 海拔图, 计算这个海拔图最多能接住多少（面积）雨水。

例如，给定一个 5*4 的矩阵：
```
  [12,13,0,12],
  [13,4,13,12],
  [13,8,10,12],
  [12,13,12,12],
  [13,13,13,13]
```
返回 14.

# 解题思路
* 维护一个最小堆，保存最外面一圈的高度，因为最矮的格子决定了水能存放多少
* 每次取最小高度 h，与周围4个中没有被访问过的元素进行比较
 * 如果该元素的高度小于 h，则注水到其中，并将其加入到最小堆中，设置该元素被访问过
 * 如果该元素的高度大于 h，则直接将其加入到最小堆中，设置改元素被访问过
* 创建一个cell类，封装x, y坐标以及高度h，还有定义compare规则
* 定义一个`dx = [1, -1, 0, 0]`和`dy = [0, 0, 1, -1]`用于生成坐标(x, y)上下左右的点

# 完整代码
```python
import Queue

class Cell:
    def __init__(self, x, y, h):
        self.x, self.y, self.h = x, y, h
        
    def __cmp__(self, other):
        if self.h < other.h:
            return -1
        elif self.h > other.h:
            return 1
        else:
            return 0

class Solution:
    # @param heights: a matrix of integers
    # @return: an integer
    def __init__(self):
        self.dx = [1,-1,0,0]
        self.dy = [0,0,1,-1]
        
    def trapRainWater(self, heights):
        if len(heights) == 0:
            return 0
        q = Queue.PriorityQueue()
        n = len(heights)
        m = len(heights[0])
        visit = [[False for i in range(m)] for i in range(n)]
        for i in range(n):
            q.put(Cell(i, 0, heights[i][0]))
            q.put(Cell(i, m-1, heights[i][m-1]))
            visit[i][0] = True
            visit[i][m-1] = True
            
        for i in range(m):
            q.put(Cell(0, i, heights[0][i]))
            q.put(Cell(n-1, i, heights[n-1][i]))
            visit[0][i] = True
            visit[n-1][i] = True
            
        ans = 0
        while not q.empty():
            now = q.get()
            for i in range(4):
                nx = now.x + self.dx[i]
                ny = now.y + self.dy[i]
                if 0 <= nx and nx < n and 0 <= ny and ny < m and not visit[nx][ny]:
                    visit[nx][ny] = True
                    q.put(Cell(nx, ny, max(now.h,heights[nx][ny])))
                    ans += max(0, now.h - heights[nx][ny])
                    
        return ans
```