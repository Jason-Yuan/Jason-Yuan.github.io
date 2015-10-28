title: "62 Unique Paths"
date: 2015-10-25 01:52:40
tags: ["LeetCode", "Dynamic Programing", "Matrix DP"]
categories: "LeetCode"
---

# 原题
>有一个机器人的位于一个M×N个网格左上角（下图中标记为'Start'）。
机器人每一时刻只能向下或者向右移动一步。机器人试图达到网格的右下角（下图中标记为'Finish'）。
问有多少条不同的路径？

| 1, 1  | 1, 2  | 1, 3  | 1, 4  |  
|:---:|:---:|:---:|:---:|
| 2, 1 |   |   |   |   
| 3, 1 |   |   |   |   
| 4, 1 |   |   | 4, 4  |   

以上4 x 4的网格中，有多少条不同的路径？

# 解题思路
* 动态规划就是解决了重复计算，具体实现方式有：
 * 记忆化搜索
 * 循环求解(本题采用此方法)
* 典型DP问题 - **矩阵型动态规划**(Matrix DP)
* `cache[x][y]`表示从起点到`x,y`的路径数，等于`cache[x][y - 1]`与`cache[x - 1][y]`的路径数之和

# 完整代码
```python
class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        if m < 1 or n < 1:
            return 0
            
        cache = [[0 for j in range(m)] for i in range(n)]
        for i in range(n):
            cache[i][0] = 1
        for j in range(m):
            cache[0][j] = 1
        for i in range(1, n):
            for j in range(1, m):
                cache[i][j] = cache[i - 1][j] + cache[i][j - 1]
                
        return cache[n - 1][m - 1]
```