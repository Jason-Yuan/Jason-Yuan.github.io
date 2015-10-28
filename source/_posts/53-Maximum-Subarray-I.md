title: "53 Maximum Subarray I"
date: 2015-09-28 21:07:55
tags: ["LeetCode", "Array", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>给定一个整数数组，找到一个具有最大和的子数组，返回其最大和。

给出数组[−2,2,−3,4,−1,2,1,−5,3]，符合要求的子数组为[4,−1,2,1]，其最大和为6
子数组最少包含一个数

# 解题思路
* 相似题[Best Time to Buy and Sell Stock]，本题是找一个区间使得区间内值的和最大，[Best Time to Buy and Sell Stock]是找两个点使得end-start最大
* 巧妙使用**前缀和数组** sum[i] = array[0] + array[1] +.... + array[i]
* **[Best Time to Buy and Sell Stock]中的每个点，就相当于[Maximum Subarray]中的前缀和数组**
* 想求x~y这段区间的最大值，则让sum[y]尽可能大，让sum[x]尽可能小，那么sum[y]-sum[x]最大

# 完整代码
```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        
        res, sum, minSum = -sys.maxint - 1, 0, 0
        for num in nums:
            sum += num
            res = max(res, sum - minSum)
            minSum = min(minSum, sum)

        return res
```