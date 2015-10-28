title: "74 Search a 2D Matrix"
date: 2015-10-11 23:29:16
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>写出一个高效的算法来搜索 m × n矩阵中的值。
这个矩阵具有以下特性：
每行中的整数从左到右是排序的。
每行的第一个数大于上一行的最后一个整数。

```
[
  [1, 3, 5, 7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
给出 target = 3，返回 true

# 解题思路
* 首先可以把这个二维数组看成一维数组，3 * 4 的矩阵可以看成一个12个数的一维数组，所以对于50的坐标为(2,3)，可以看成第array[11]
```
[1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 50]
```
* 一维数组与二维数组的转化
```
2 = 11 / 4, 3 = 11 % 4   # width = 4
``` 

# 完整代码
```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix:
            return False
        
        start = 0
        width = len(matrix[0])
        end = len(matrix) * width - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if matrix[mid / width][mid % width] == target:
                return True
            elif matrix[mid / width][mid % width] > target:
                end = mid
            else:
                start = mid
            
        if matrix[start / width][start % width] == target:
            return True
        if matrix[end / width][end % width] == target:
            return True
        return False
```