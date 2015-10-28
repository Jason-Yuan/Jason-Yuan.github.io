title: "42 Maximum Subarray II"
date: 2015-09-28 23:43:51
tags: ["LintCode", "Array"]
categories: "LintCode"
---

# 原题
>给定一个整数数组，找出两个**不重叠**子数组使得它们的和最大。
每个子数组的数字在数组中的位置应该是连续的。
返回最大的和。

给出数组**[1, 3, -1, 2, -1, 2]**，这两个子数组分别为**[1, 3]**和**[2, -1, 2]**或者**[1, 3, -1, 2]**和**[2]**，它们的最大和都是7

子数组最少包含一个数

# 解题思路
* 类似题是[Best Time to Buy and Sell Stock III]
* 找两个区间，和最大，则一定存在一个分割线，分割线左右两边分别转化为求一个子数组最大，即[Maximum Subarray I]
* 从左往右枚举一遍分割线求出Maximum Subarray，再从右往左枚举一遍分割线求出Maximum Subarray 这样时间复杂度是O(n)
* 如果从左往右枚举一次分割线，每次左右分别求maximum Subarray，过程中是有浪费的，时间复杂度是O(n^2)

# 完整代码
```python
class Solution:
    """
    @param nums: A list of integers
    @return: An integer denotes the sum of max two non-overlapping subarrays
    """
    def maxTwoSubArrays(self, nums):
        if not nums:
            return 0
            
        size = len(nums)
        left, right = [0 for i in range(size)], [0 for i in range(size)]
        res, sum, minSum = -sys.maxint - 1, 0, 0
        for i in range(size):
            sum += nums[i]
            res = max(res, sum - minSum)
            minSum = min(minSum, sum)
            left[i] = res
            
        res, sum, minSum = -sys.maxint - 1, 0, 0
        for i in range(size - 1, -1, -1):
            sum += nums[i]
            res = max(res, sum - minSum)
            minSum = min(minSum, sum)
            right[i] = res
        
        result = -sys.maxint - 1
        for i in range(size - 1):
            result = max(result, left[i] + right[i + 1])
            
        return result
```