title: "152 Maximum Product Subarray"
date: 2015-09-30 00:40:58
tags: ["LeetCode", "Array", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>找出一个序列中乘积最大的连续子序列（至少包含一个数）。

比如, 序列 [2,3,-2,4]中乘积最大的子序列为 [2,3]，其乘积为6。

## 解题思路
* 动态规划求解
* 维护两个数组maxCache和minCache, 每次判断下一个数的正负
```python
# 如果是正数 
maxCache = max(maxCache[i], maxCache[i - 1] * nums[i])
minCache[i] = min(minCache[i], minCache[i - 1] * nums[i])
# 如果是负数
maxCache[i] = max(maxCache[i], minCache[i - 1] * nums[i])
minCache[i] = min(minCache[i], maxCache[i - 1] * nums[i])
```
* result变量去维护一个最大值

# 完整代码
```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        maxCache = [0 for i in range(len(nums))]
        minCache = [0 for i in range(len(nums))]
        
        minCache[0] = maxCache[0] = nums[0];
        result = nums[0]
        for i in range(1, len(nums)):
            minCache[i] = maxCache[i] = nums[i]
            if nums[i] > 0:
                maxCache[i] = max(maxCache[i], maxCache[i - 1] * nums[i])
                minCache[i] = min(minCache[i], minCache[i - 1] * nums[i])
            elif nums[i] < 0:
                maxCache[i] = max(maxCache[i], minCache[i - 1] * nums[i])
                minCache[i] = min(minCache[i], maxCache[i - 1] * nums[i])
            
            result = max(result, maxCache[i])
        
        return result
```