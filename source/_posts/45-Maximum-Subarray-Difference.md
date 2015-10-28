title: "45 Maximum Subarray Difference"
date: 2015-09-29 01:23:41
tags: ["LintCode", "Array"]
categories: "LintCode"
---

# 原题
>给定一个整数数组，找出两个**不重叠**的子数组A和B，使两个子数组和的差的绝对值**|SUM(A) - SUM(B)|**最大。
返回这个最大的差值。

给出数组**[1, 2, -3, 1]**，返回 6

子数组最少包含一个数

# 解题思路
* 类似题[Maximum Subarray II]
* 枚举分割线，但本题每次要知道分割线左边和右边的**最大/最小数组和**
* 枚举一遍分割线，求max( abs(左最大-右最小), abs(左最小-右最大) )


# 完整代码
```python
class Solution:
    """
    @param nums: A list of integers
    @return: An integer indicate the value of maximum difference between two
             Subarrays
    """
    def maxDiffSubArrays(self, nums):
        if not nums:
            return 0
            
        size = len(nums)
        leftMax, leftMin = [0 for i in range(size)], [0 for i in range(size)]
        rightMax, rightMin = [0 for i in range(size)], [0 for i in range(size)]
        maxRes, minRes, sum, minSum, maxSum = -sys.maxint - 1, sys.maxint, 0, 0, 0
        for i in range(size):
            sum += nums[i]
            maxRes = max(maxRes, sum - minSum)
            minSum = min(minSum, sum)
            leftMax[i] = maxRes
            minRes = min(minRes, sum - maxSum)
            maxSum = max(maxSum, sum)
            leftMin[i] = minRes
            
        maxRes, minRes, sum, minSum, maxSum = -sys.maxint - 1, sys.maxint, 0, 0, 0
        for i in range(size - 1, -1, -1):
            sum += nums[i]
            maxRes = max(maxRes, sum - minSum)
            minSum = min(minSum, sum)
            rightMax[i] = maxRes
            minRes = min(minRes, sum - maxSum)
            maxSum = max(maxSum, sum)
            rightMin[i] = minRes
        
        result = -sys.maxint - 1
        for i in range(size - 1):
            result = max(result, abs(leftMax[i] - rightMin[i + 1]), abs(leftMin[i] - rightMax[i + 1]))
            
        return result
```