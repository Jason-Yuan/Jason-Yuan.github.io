title: "55 Jump Game"
date: 2015-10-26 03:34:39
tags: ["LeetCode", "Sequence DP", "Greedy", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>给出一个非负整数数组，你最初定位在数组的第一个位置。　　　
数组中的每个元素代表你在那个位置可以跳跃的最大长度。　　　　
判断你是否能到达数组的最后一个位置。

A =** [2,3,1,1,4]**，返回 true.
A =** [3,2,1,0,4]**，返回 false.

# 解题思路
* 方法一：贪心法 greedy
* 方法二：单序列型动态规划 - Sequence DP (因为题目中给的是序列而不是数组，注意两者的区别)，本题动归的方法不能过OJ


# 完整代码
```python
# 方法一，贪心
class Solution(object):
    def canJump(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        if not nums:
            return False
        
        farest = nums[0]
        for i in range(len(nums)):
            if i <= farest and nums[i] + i > farest:
                farest = nums[i] + i
            if farest >= len(nums) - 1:
                return True
        return False

# 方法二，动态规划
class Solution(object):
    def canJump(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        cache = [False for i in range(len(nums))]
        cache[0] = True
        for i in range(len(nums)):
            for j in range(i):
                if cache[j] is True and j + nums[j] >= i:
                    cache[i] == True
                    break
        return cache[len(nums) - 1]
```