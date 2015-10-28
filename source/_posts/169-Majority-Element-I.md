title: "169 Majority Element I"
date: 2015-09-27 23:47:03
tags: ["LeetCode", "Array"]
categories: "LeetCode"
---

# 原题
>给定一个整型数组，找出主元素，它在数组中的出现次数严格大于数组元素个数的二分之一。

给出数组**[1,1,1,1,2,2,2]**，返回 1

# 解题思路
* 方法一，排序，返回中位数 O(nlogn)
* 方法二，遍历数组一次，不一样的相互抵消，数量占优的会留下来 O(n)

## 完整代码
```python
# Method 1
class Solution:
    # @param {integer[]} nums
    # @return {integer}
    def majorityElement(self, nums):
        return sorted(nums)[len(nums)/2]
 
# Method 2
class Solution:
    # @param {integer[]} nums
    # @return {integer}
    def majorityElement(self, nums):
        count, candidate = 0, -1
        for item in nums:
            if count == 0:
                candidate = item
                count = 1
            elif candidate != item:
                count -= 1
            else:
                count += 1
        return candidate
```