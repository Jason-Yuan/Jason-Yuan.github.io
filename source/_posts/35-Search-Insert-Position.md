title: "35 Search Insert Position"
date: 2015-10-11 22:51:42
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>给定一个排序数组和一个目标值，如果在数组中找到目标值则返回索引。如果没有，返回到它将会被按顺序插入的位置。

[1,3,5,6]，5 → 2
[1,3,5,6]，2 → 1
[1,3,5,6]， 7 → 4
[1,3,5,6]，0 → 0

# 解题思路
* 对于插入排序，所有数字都要插入一次，而且对于数组，要整体向后移动，所以这时的插入即从后往前交换到不能交换位置。**二分查找再插入并不会加快速度**
* 本题是假设有一个很大的数组，数组之间可能留有空隙，只需要插入一个或几个数字，所以需要**二分法**快速定位
* **概念偷换**可以减少if/else判断语句！！！本题把target存在和不存在结合在一起 => **找到数组中有几个数比target小**
* 核心部分都写成`start = mid`或者`end = mid`，具体情况最后讨论

# 完整代码
```python
class Solution:
    """
    @param A : a list of integers
    @param target : an integer to be inserted
    @return : an integer
    """
    def searchInsert(self, A, target):
        if not A:
            return 0
            
        if target < A[0]:
            return 0
            
        start, end = 0, len(A) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if A[mid] == target:
                return mid
            elif A[mid] < target:
                start = mid
            else:
                end = mid
                
        if A[end] == target:
            return end
        elif A[end] < target:
            return end + 1
        elif A[start] == target:
            return start
        else:
            return end
```