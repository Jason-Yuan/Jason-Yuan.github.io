title: "14 Binary Search"
date: 2015-10-11 22:00:52
tags: ["LintCode", "Array", "Binary Search"]
categories: "LintCode"
---

# 原题
>给定一个排序的整数数组（升序）和一个要查找的整数target，用O(logn)的时间查找到target第一次出现的下标（从0开始），如果target不存在于数组中，返回-1。

在数组 [1, 2, 3, 3, 4, 5, 10] 中二分查找3，返回2。

# 解题思路
* 首先使用**start + 1 < end** =>表示相交或者相邻停止
* 因为是找第一次出现的位置所以
```
if nums[mid] < target:
    start = mid
else:
    end = mid
```
* 最后start和end可能相等可能不等，统一模板，两个都check，顺序依照题目而定，本题先chcek start

# 完整代码
```python
class Solution:
    # @param nums: The integer array
    # @param target: Target number to find
    # @return the first position of target in nums, position start from 0 
    def binarySearch(self, nums, target):
        if not nums:
            return -1
            
        start, end = 0, len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if nums[mid] < target:
                start = mid
            else:
                end = mid
                
        if nums[start] == target:
            return start
        if nums[end] == target:
            return end
        return -1
```