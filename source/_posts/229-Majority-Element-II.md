title: "229 Majority Element II"
date: 2015-09-27 23:48:19
tags: ["LeetCode", "Array"]
categories: "LeetCode"
---

# 原题
>给定一个整型数组，找到所有主元素，它在数组中的出现次数严格大于数组元素个数的三分之一。

给出数组**[1,2,1,2,1,3,3] **返回 1

# 解题思路
* 三三抵消，最后会剩下两个candidates，但是注意此时不是谁占多数谁是最终结果，反例[1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 4, 4] 三三抵消后剩下 [1, 4，4] 4数量占优，但结果应该是1，所以三三抵消后，再loop一遍找1和4谁数量超过了len(nums)/3

# 完整代码
```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        candidate1, count1 = None, 0
        candidate2, count2 = None, 0
        for num in nums:
            if candidate1 == num:
                count1 += 1
            elif candidate2 == num:
                count2 += 1
            elif count1 == 0:
                count1 += 1
                candidate1 = num
            elif count2 == 0:
                count2 += 1
                candidate2 = num
            else:
                count1 -= 1
                count2 -= 1
                
        count1, count2 = 0, 0
        for num in nums:
            if num == candidate1:
                count1 += 1
            if num == candidate2:
                count2 += 1
                
        result = []
        if count1 > len(nums) / 3:
            result.append(candidate1)
        if count2 > len(nums) / 3:
            result.append(candidate2)
        return result
```