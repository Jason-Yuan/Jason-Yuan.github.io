title: "63 Unique Paths II"
date: 2015-10-25 02:18:09
tags: ["LeetCode", "Dynamic Programing", "Matrix DP"]
categories: "LeetCode"
---

# 原题
>现在考虑网格中有障碍物，那样将会有多少条不同的路径？
网格中的障碍和空位置分别用**1**和**0**来表示。

如下所示在3x3的网格中有一个障碍物：
```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
一共有2条不同的路径从左上角到右下角。

# 解题思路
* 同样是矩阵型动态规划 - Matrix DP
* 对于有障碍物的地图来说，只需要把**障碍物的坐标设为0即可**
* 要注意对第一行和第一列的初始化，如果遇到障碍物第一行后面和第一列下面都是0

# 完整代码
```python
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        if not obstacleGrid or not obstacleGrid[0]:
            return 0
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        cache = [[0 for i in range(n)] for j in range(m)]
        flag1 = flag2 = 1
        for j in range(m): 
            if obstacleGrid[j][0] == 1:
                flag1 = 0
            cache[j][0] = flag1
        for i in range(n):
            if obstacleGrid[0][i] == 1:
                flag2 = 0
            cache[0][i] = flag2
        for j in range(1, m):
            for i in range(1, n):
                cache[j][i] = (cache[j - 1][i] + cache[j][i - 1]) if obstacleGrid[j][i] == 0 else 0
        return cache[m - 1][n - 1]
```