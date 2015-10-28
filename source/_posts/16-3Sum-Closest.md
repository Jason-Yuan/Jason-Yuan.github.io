title: "16 3Sum Closest"
date: 2015-10-02 04:29:15
tags: ["LeetCode", "Array", "Two Pointers", "Sum"]
categories: "LeetCode"
---

# 原题
>给一个包含n个整数的数组S, 找到和与给定整数target最接近的三元组，返回这三个数的和。

例如S = **[-1, 2, 1, -4]** and target = **1**.  和最接近1的三元组是 **-1 + 2 + 1 = 2**.

只需要返回三元组之和，无需返回三元组本身

# 解题思路
* 题目要求找到x+y+z最接近target，我们可以遍历x，然后将题目转化为2Sum Closest，即找到y+z最接近target-x
* 对于2Sum Closest，同样是使用**对撞型指针**，不断维护一个**closest sum**

# 完整代码
```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums:
            return []
            
        sortNum = sorted(nums)
        closest = sys.maxint
        for i in range(len(sortNum)):
            left, right = i + 1, len(sortNum) - 1
            while left < right:
                Sum = sortNum[i] + sortNum[left] + sortNum[right]
                if Sum == target:
                    return Sum
                elif Sum > target:
                    right -= 1
                else:
                    left += 1
                closest = Sum if abs(Sum - target) < abs(closest - target) else closest 
                
        return closest
```