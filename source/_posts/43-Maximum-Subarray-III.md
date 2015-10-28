title: "43 Maximum Subarray III"
date: 2015-10-04 17:44:42
tags: ["LintCode", "Array", "Dynamic Programing"]
categories: "LintCode"
---

# 原题
>给定一个整数数组和一个整数k，找出**k**个不重叠子数组使得它们的和最大。
每个子数组的数字在数组中的位置应该是连续的。
返回最大的和。

给出数组**[-1,4,-2,3,-2,3]**以及k=2，返回 8

子数组最少包含一个数

# 解题思路
* 典型划分类动态规划
* localMax[i][k] 表示前i个数，取k个子数组，包含第i个数的Maximum Sum
* globalMax[i][k] 表示前i个数，取k个子数组，可以不包含第i个数的Maximum Sum
* 状态转移方程
```python
localMax[i][k] = max(localMax[i - 1][k] + nums[i - 1], globalMax[i - 1][k - 1] + nums[i - 1])
globalMax[i][k] = max(globalMax[i - 1][k], localMax[i][k])
```

# 完整代码
```python
class Solution:
    """
    @param nums: A list of integers
    @param k: An integer denote to find k non-overlapping subarrays
    @return: An integer denote the sum of max k non-overlapping subarrays
    """
    def maxSubArray(self, nums, k):
        if not nums:
            return 0
            
        n = len(nums)
        localMax = [[-sys.maxint for i in range(k + 1)] for i in range(n + 1)]
        globalMax = [[-sys.maxint for i in range(k + 1)] for i in range(n + 1)]
            
        # 边界初始化
        for k in range(k + 1):
            localMax[k][0] = globalMax[k][0] = 0
            
        for i in range(1, n + 1):
            # 边界初始化
            localMax[i][0] = 0
            globalMax[i][0] = 0
            for k in range(1, k + 1):
                localMax[i][k] = max(localMax[i - 1][k] + nums[i - 1], globalMax[i - 1][k - 1] + nums[i - 1])
                globalMax[i][k] = max(globalMax[i - 1][k], localMax[i][k])
        
        return globalMax[n][k]
```