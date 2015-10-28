title: "44 Minimum Subarray"
date: 2015-09-28 21:17:30
tags: ["LintCode", "Array", "Dynamic Programing"]
categories: "LintCode"
---

# 原题
>给定一个整数数组，找到一个具有最小和的子数组。返回其最小和。

给出数组**[1, -1, -2, 1]**，返回 -3
子数组最少包含一个数字

# 解题思路
* 相似题[Maximum Subarray]，本题是找一个区间使得区间内值的和最小，[Maximum Subarray]是找一个区间使得区间内值的和最大
* 巧妙使用**前缀和数组** sum[i] = array[0] + array[1] +.... + array[i]
* 想求x~y这段区间的最小值，则让sum[y]尽可能小，让sum[x]尽可能大，那么sum[y]-sum[x]最小

# 完整代码
```python
class Solution:
    """
    @param nums: a list of integers
    @return: A integer denote the sum of minimum subarray
    """
    def minSubArray(self, nums):
        if not nums:
            return 0
        
        res, sum, maxSum = sys.maxint - 1, 0, 0
        for num in nums:
            sum += num
            res = min(res, sum - maxSum)
            maxSum = max(maxSum, sum)

        return res
```