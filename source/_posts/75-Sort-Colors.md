title: "75 Sort Colors"
date: 2015-10-02 16:37:43
tags: ["LeetCode", "Array", "Two Pointers", "Quick Sort"]
categories: "LeetCode"
---

# 原题
>给定一个包含红，白，蓝且长度为n的数组，将数组元素进行分类使同颜色的元素相邻，并按照红、白、蓝的顺序进行排序。
我们可以使用整数0，1和2分别代表红，白，蓝。

一个相当直接的解决方案是使用计数排序扫描2遍的算法。
首先，迭代数组计算0,1,2出现的次数，然后依次用0,1,2出现的次数去覆盖数组。
你否能想出一个仅使用常数级额外空间复杂度且只扫描遍历一遍数组的算法？

# 解题思路
* 最直接的想法是类似于桶排序，先loop一遍计算每种颜色的个数，再一遍从0->2按照相应个数重新生成数组，如下：
```python
def sortColors(self, nums): 
    nums[:] = [0]*nums.count(0) + [1]*nums.count(1) + [2]*nums.count(2)
```
* 本题，根据要求，一遍扫描 -> 拿到一个元素，就应该知道该放的位置
* 借助快速排序中partition思想，把1当作pivot元对数组进行划分，使0在数组的左边，2在数组的右边，1在数组的中间。
* 记录红色index=0，蓝色index=n，loop数组，每次判断颜色，红色则与红色index交换，蓝色与蓝色index交换，黄色不变      

# 完整代码
```python
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        if not nums:
            return
        
        i = 0
        redIndex, blueIndex = 0, len(nums) - 1
        while i <= blueIndex:
            if nums[i] == 0:
                nums[i], nums[redIndex] = nums[redIndex], nums[i]
                i += 1
                redIndex += 1
                continue
            elif nums[i] == 2:
                nums[i], nums[blueIndex] = nums[blueIndex], nums[i]
                blueIndex -= 1
                continue
            i += 1
```