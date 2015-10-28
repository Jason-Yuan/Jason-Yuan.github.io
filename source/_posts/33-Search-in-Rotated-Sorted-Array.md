title: "33 Search in Rotated Sorted Array"
date: 2015-10-12 03:31:40
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>假设有一个排序的按未知的旋转轴旋转的数组(比如，0 1 2 4 5 6 7 可能成为4 5 6 7 0 1 2)。给定一个目标值进行搜索，如果在数组中找到目标值返回数组中的索引位置，否则返回-1。

给出[4, 5, 1, 2, 3]和target=1，返回 2
给出[4, 5, 1, 2, 3]和target=0，返回 -1

# 解题思路
* **没有重复的元素**是重要信息，有重复下面方法不成立
* 排序数组 => binary search => 每次扔掉一半，找中间的数
```
       /|
     /  |
   /    |
 ______|_____
        |    /
        |  /
        | /
```
* 分情况讨论Mid落在左上区间还是右下区间，然后再分别讨论target跟Mid的关系
```python
if nums[mid] > nums[start]:
    # mid 在左上
else:
    # mid 在右下
```
# 完整代码
```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums:
            return -1
            
        start, end = 0, len(nums) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if nums[mid] == target:
                return mid
            elif nums[mid] > nums[start]:
                # 注意此处有两个等于号，表明在闭区间内
                if target >= nums[start] and target <= nums[mid]:
                    end = mid
                else:
                    start = mid
            else:
                # 注意此处有两个等于号，表明在闭区间内
                if target >= nums[mid] and target <= nums[end]:
                    start = mid
                else:
                    end = mid
                    
        if nums[start] == target:
            return start
        if nums[end] == target:
            return end
        return -1
```