title: "154 Find Minimum in Rotated Sorted Array II"
date: 2015-10-12 17:11:08
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>假设一个旋转排序的数组其起始位置是未知的（比如0 1 2 4 5 6 7 可能变成是4 5 6 7 0 1 2）。

你需要找到其中最小的元素。
数组中可能存在重复的元素。
给出[4,4,5,6,7,0,1,2]  返回 0

# 解题思路
* 与[Find Minimum in Rotated Sorted Array I]相比，本题中有**重复元素**，还是让`nums[mid]`和`nums[end]`比较，但要考虑`nums[mid] == mums[end]`的情况，此时 `end -= 1`
* 极端情况[1, 2, 2, 2, .....2]，`nums[mid]`和`mums[end]`一直相等，操作一直是`end -= 1`，所以时间复杂度变成O(n)
* 所以方法二就是直接遍历一遍，维护一个最小值
* 对于方法一，还可以想成在普通的binary search基础上，加入两个while循环
```python 
while start + 1 < end:
    mid = start + (end - start) / 2
    # 如果重复，start向后移动
    while nums[mid] == nums[start] and start < mid:
        start += 1
    # 如果重复，end向前移动
    while nums[mid] == nums[end] and end > mid:
        end -= 1
    if nums[mid] > nums[end]:
        start = mid
    elif nums[mid] < nums[end]:
        end = mid
```

# 完整代码
```python
# 方法一
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
            elif nums[mid] < nums[end]:
                end = mid
            else:
                end -= 1
        return min(nums[start], nums[end])

# 方法二
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return
        min = nums[0]
        for item in nums:
            if item < min:
                min = item
        return min
```