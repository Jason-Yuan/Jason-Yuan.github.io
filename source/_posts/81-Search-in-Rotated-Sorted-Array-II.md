title: "81 Search in Rotated Sorted Array II"
date: 2015-10-12 14:29:50
tags: ["LeetCode", "Array", "Binary Search"]
categories: "LeetCode"
---

# 原题
>跟进“搜索旋转排序数组”，假如有重复元素又将如何？

给出[3,4,4,5,7,0,1,2]和target=4，返回 true

# 解题思路
* 本题有重复元素，不能使用binary search
* 极端例子，[1, 1, 1, 1, 1, 1, 2, 1, 1, 1]中找2的情况

# 完整代码
```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        for num in nums:
            if num == target:
                return True
        return False
```