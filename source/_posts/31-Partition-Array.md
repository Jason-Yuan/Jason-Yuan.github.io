title: "31 Partition Array"
date: 2015-09-30 01:01:21
tags: ["LintCode", "Array", "Two Pointers"]
categories: "LintCode"
---

# 原题
>给出一个整数数组nums和一个整数k。划分数组（即移动数组nums中的元素），使得：
所有小于k的元素移到左边
所有大于等于k的元素移到右边
返回数组划分的位置，即数组中第一个位置i，满足nums[i]大于等于k。

给出数组nums=**[3,2,2,1]**和 k=2，返回 1

# 解题思路
* 非常相似的题[Partition Array by Odd and Even]
* 正宗**两个指针**中对撞型指针的问题
* 最外层大while循环，终止条件是两根指针对撞
* 内层循环，分三步走
 * 左边如果小于K, left++
 * 右边如果大于等于K, right--
 * 最后停下时如果left <= right, swap 

# 完整代码
```python
class Solution:
    """
    @param nums: The integer array you should partition
    @param k: As description
    @return: The index after partition
    """
    def partitionArray(self, nums, k):
        # write your code here
        # you should partition the nums by k
        # and return the partition index as description
        if not nums:
            return 0
            
        left, right = 0, len(nums) - 1
        while left <= right:
            while left <= right and nums[left] < k:
                left += 1
            while left <= right and nums[right] >= k:
                right -= 1
            if left <= right:
                nums[left],  nums[right] = nums[right], nums[left]
                left += 1
                right -= 1
        return left
```