title: "64 Minimum Path Sum"
date: 2015-10-25 03:32:25
tags: ["LeetCode", "Dynamic Programing", "Matrix DP"]
categories: "LeetCode"
---

# 原题
>给定一个只含非负整数的m*n网格，找到一条从左上角到右下角的可以使数字和最小的路径。

你在同一时间只能向下或者向右移动一步

# 解题思路
* 矩阵型动态规划 - Matrix DP
* 与Unique Paths I/II非常类似，不同的是每次要找最小value
* cache[i][j]表示从左上角到达i,j的数字和最小路径，它等于cache[i][j - 1]与cache[i - 1][j]的最小值，加上grid[i][j]的值

# 完整代码
```python
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if not grid or not grid[0]:
            return 0
        m = len(grid)
        n = len(grid[0])
        cache = [[0 for i in range(n)] for j in range(m)]
        sum1 = sum2 = 0
        for i in range(n):
            sum1 += grid[0][i]
            cache[0][i] = sum1
        for j in range(m):
            sum2 += grid[j][0]
            cache[j][0] = sum2
        for i in range(1, n):
            for j in range(1, m):
                cache[j][i] = min(cache[j - 1][i], cache[j][i - 1]) + grid[j][i]
        return cache[m - 1][n - 1]
```