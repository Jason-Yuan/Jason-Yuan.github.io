title: "34 Search for a Range"
date: 2015-10-12 02:50:19
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>给定一个包含 n 个整数的排序数组，找出给定目标值 target 的起始和结束位置。
如果目标值不在数组中，则返回[-1, -1]

给出[5, 7, 7, 8, 8, 10]和目标值 target=8,
返回[3, 4]

# 解题思路
* 错误思路：找到一个target，然后向前向后搜索边界，如果整个数组都是target，则时间复杂度变为O(n)
* 正确思路：**分两步**，找到target的first position和last position
* 类似的题，**一个sorted array中target出现了几次**，一样的解法

# 完整代码
```python
class Solution:
    """
    @param A : a list of integers
    @param target : an integer to be searched
    @return : a list of length 2, [index1, index2]
    """
    def searchRange(self, A, target):
        if not A or target > A[-1] or target < A[0]:
            return [-1, -1]
            
        # find left bound
        start, end = 0, len(A) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if A[mid] < target:
                start = mid
            else:
                end = mid
        if A[start] == target:
            leftBound = start
        elif A[end] == target:
            leftBound = end
        else:
            return [-1, -1]
            
        # find right bound
        start, end = 0, len(A) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if A[mid] > target:
                end = mid
            else:
                start = mid
        if A[end] == target:
            rightBound = end
        elif A[start] == target:
            rightBound = start
        else:
            return [-1, -1]
            
        return [leftBound, rightBound]        
```