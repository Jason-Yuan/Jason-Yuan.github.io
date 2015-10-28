title: "139 Subarray Sum Closest"
date: 2015-10-03 03:03:13
tags: ["LintCode", "Array", "Sort"]
categories: "LintCode"
---

# 原题
>给定一个整数数组，找到一个和最接近于零的子数组。返回第一个和最有一个指数。你的代码应该返回满足要求的子数组的起始位置和结束位置。

给出**[-3, 1, 1, -3, 5]**，返回**[0, 2]**，**[1, 3]**，**[1, 1]**，**[2, 2]**或者**[0, 4]**

O(nlogn)的时间复杂度

# 解题思路
* 首先建立一个pair类，便于记录前n项和与对应的index。因为后面要对sum数组排序，结果又要返回index
* loop一遍有序的sum数组，维护一个Closest变量，有更小的就更新

# 完整代码
```python
class Pair:
    def __init__(self, sum, index):
        self.sum = sum
        self.index = index
        
class Solution:
    """
    @param nums: A list of integers
    @return: A list of integers includes the index of the first number 
             and the index of the last number
    """
    def subarraySumClosest(self, nums):
        res = []
        if not nums:
            return res
            
        length = len(nums)
        if length == 1:
            return [0, 0]
            
        sums = [Pair(0, 0)]
        prev = 0
        for i in range(1, length + 1):
            sums.append(Pair(prev + nums[i - 1], i))
            prev = sums[i].sum
            
        sums = sorted(sums, key=lambda pair : pair.sum)
        Closest = sys.maxint
        for i in range(1, length + 1):
            if Closest > sums[i].sum - sums[i - 1].sum:
                Closest = sums[i].sum - sums[i - 1].sum
                res = []
                tmp = [sums[i].index - 1, sums[i - 1].index - 1]
                tmp.sort()
                res.append(tmp[0] + 1)
                res.append(tmp[1])
                
        return res
```