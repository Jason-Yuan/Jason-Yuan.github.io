title: "373 Partition Array by Odd and Even"
date: 2015-09-30 00:42:20
tags: ["LintCode", "Two Pointers", "Array"]
categories: "LintCode"
---

# 原题
>分割一个整数数组，使得奇数在前偶数在后。

给定 [1, 2, 3, 4]，返回 [1, 3, 2, 4]。

# 解题思路
* 非常相似的题[Partition Array]
* 正宗**两个指针**中对撞型指针的问题
* 最外层大while循环，终止条件是两根指针对撞
* 内层循环，分三步走
 * 左边如果是奇数, left++
 * 右边如果是偶数, right--
 * 最后停下时如果left <= right, swap

# 完整代码
```python
class Solution:
    # @param nums: a list of integers
    # @return: nothing
    def partitionArray(self, nums):
        if not nums:
            return nums
            
        left, right = 0, len(nums) - 1
        while left <= right:
            while left <= right and nums[left] % 2 == 1:
                left += 1
            while left <= right and nums[right] % 2 == 0:
                right -= 1
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right -= 1
```