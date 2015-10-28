title: "240 Search a 2D Matrix II"
date: 2015-10-12 15:09:32
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>写出一个高效的算法来搜索m×n矩阵中的值，返回这个值是否出现。
这个矩阵具有以下特性：
每行中的整数从左到右是排序的。
每一列的整数从上到下是排序的。
在每一行或每一列中没有重复的整数。

考虑下列矩阵：
```
[
    [1, 3, 5, 7],
    [2, 4, 7, 8],
    [3, 5, 9, 10]
]
```
给出target = 3，返回 True

# 解题思路
* 思考时间复杂度时，可以从有多少个答案的角度考虑，像permutation时间复杂度肯定是2^n而不会是n^2
* [Search a 2D Matrix I]是每一行的最后一个都小于下一行的第一个，所以可以把整个二维数组看成递增的一维数组，而本题不同。
* 把起始点设为左下角，如本题，如果target > 3则第一列跳过，如果target < 3则最后一行跳过

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
            return 0
            
        depth = len(matrix)
        width = len(matrix[0])
        row, col = depth - 1, 0
        while row >= 0 and col < width:
            if matrix[row][col] == target:
                return True
            elif matrix[row][col] > target:
                row -= 1
            else:
                col += 1
        return False
```