title: "120 Triangle"
date: 2015-10-26 03:02:53
tags: ["LeetCode", "Matrix DP", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>给定一个数字三角形，找到从顶部到底部的最小路径和。每一步可以移动到下面一行的相邻数字上。

比如，给出下列数字三角形：
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
从顶到底部的最小路径和为11 (** 2 + 3 + 5 + 1 = 11**)。

# 解题思路
* 矩阵型动态规划问题 - Matrix DP
* 有两种解决方案，自顶向下或者自底向上
* 下面代码采用自底向上的方法，cache[i][j]表明从最底层到i,j所用的最短距离是多少
* `cache[i][j]`就等于`cache[i+1][j]`(左下角)与`cache[i+1][j+1]`(右下角)中最小的加上`triangle[i][j]`的值

# 完整代码
```python
class Solution(object):
    def minimumTotal(self, triangle):
        """
        :type triangle: List[List[int]]
        :rtype: int
        """
        if not triangle:
            return 0
        
        m = len(triangle)
        cache = [[0 for i in range(m)] for j in range(m)]
        for i in range(m):
            cache[m - 1][i] = triangle[m - 1][i]
            
        for i in range(1, m):
            for j in range(m - i):
                cache[m - 1 - i][j] = min(cache[m - i][j], cache[m - i][j + 1]) + triangle[m - 1 - i][j]
                
        return cache[0][0]
```