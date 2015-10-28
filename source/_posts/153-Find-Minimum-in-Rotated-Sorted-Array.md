title: "153 Find Minimum in Rotated Sorted Array"
date: 2015-10-12 16:24:55
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
 >假设一个旋转排序的数组其起始位置是未知的（比如0 1 2 4 5 6 7 可能变成是4 5 6 7 0 1 2）。
你需要找到其中最小的元素。
你可以假设数组中不存在重复的元素。

给出[4,5,6,7,0,1,2]  返回 0

# 解题思路
* 如果极端情况数组旋转了0次，即仍未升序数组，我们可以通过
```python
if nums[mid] < nums[end]:
    end = mid
```
使得最后start = 0，end = 1，返回min(A[start], A[end])
* 对于旋转的情况，有可能出现
```python
if nums[mid] > nums[end]:
    start = mid
```
最后也只需要比较一下A[start], A[end]

# 完整代码
```python
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
            
        start, end = 0, len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if nums[mid] > nums[end]:
                start = mid
            else:
                end = mid
        return min(nums[start], nums[end])
```