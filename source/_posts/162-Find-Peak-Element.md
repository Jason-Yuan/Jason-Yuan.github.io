title: "162 Find Peak Element"
date: 2015-10-12 20:18:11
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>你给出一个整数数组(size为n)，其具有以下特点：
相邻位置的数字是不同的
A[0] < A[1] 并且 A[n - 2] > A[n - 1]
假定P是峰值的位置则满足A[P] > A[P-1]且A[P] > A[P+1]，返回数组中任意一个峰值的位置。

给出数组[1, 2, 1, 3, 4, 5, 7, 6]返回1, 即数值 2 所在位置, 或者6, 即数值 7 所在位置.

# 解题思路
* 分三种情况讨论
 * nums[mid] < nums[mid - 1]说明mid往前是一个递增区间，存在峰值
 * nums[mid] < nums[mid + 1]说明mid往后是一个递增区间，存在峰值
 * 第三种情况mid比左右都大，mid就是峰，返回mid

# 完整代码
```python
class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return -1
            
        start, end = 0, len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if nums[mid] < nums[mid - 1]:
                end = mid
            elif nums[mid] < nums[mid + 1]:
                start = mid
            else:
                return mid
        return start if nums[start] > nums[end] else end
```