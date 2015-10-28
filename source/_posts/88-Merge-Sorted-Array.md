title: "88 Merge Sorted Array"
date: 2015-10-12 15:50:35
tags: ["LeetCode", "Array", "Merge"]
categories: "LeetCode"
---

# 原题
>合并两个排序的整数数组A和B变成一个新的数组。

给出A = [1, 2, 3, empty, empty] B = [4,5]
合并之后A将变成[1,2,3,4,5]
你可以假设A具有足够的空间（A数组的大小大于或等于m+n）去添加B中的元素

# 解题思路
* 从A的最大的元素和B的最大的元素开始比较
* 三个指针向前移动
```python
lastNum1 = m - 1
lastNum2 = n - 1
lastRes = len(nums1) - 1
```

# 完整代码
```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: void Do not return anything, modify nums1 in-place instead.
        """
        lastNum1 = m - 1
        lastNum2 = n - 1
        lastRes = len(nums1) - 1
        while lastNum1 >= 0 and lastNum2 >= 0:
            if nums1[lastNum1] >= nums2[lastNum2]:
                nums1[lastRes] = nums1[lastNum1]
                lastNum1 -= 1
                lastRes -= 1
            else:
                nums1[lastRes] = nums2[lastNum2]
                lastNum2 -= 1
                lastRes -= 1
        while lastNum2 >= 0:
            nums1[lastRes] = nums2[lastNum2]
            lastNum2 -= 1
            lastRes -= 1
```