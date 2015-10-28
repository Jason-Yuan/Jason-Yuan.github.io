title: "42 Trapping Rain Water"
date: 2015-10-06 19:59:50
tags: ["LeetCode", "Array", "Two Pointer", "Stack"]
categories: "LeetCode"
---

# 原题
>给出 n 个非负整数，代表一张X轴上每个区域宽度为 1 的海拔图, 计算这个海拔图最多能接住多少（面积）雨水。

海拔分别为 [0,1,0,2,1,0,1,3,2,1,2,1], 返回 6.

# 解题思路
* 一块直柱能接的水取决于左右两边较短的高度，所以第一个比较直观的方法是从左到有遍历一遍记录该点左边的最大值，从右到左遍历一遍记录该点的右边的最大值，此方法为O(n) 时间, O(n) 空间，第二种方法可以利用**单调递减栈**来实现，此方法为O(n) 时间, O(n) 空间
* 第三种方法为先找出中间最大点，然后左边部分相当于已经确定了右边最大值，左指针不断向右逼近就行，右边部分相当于记录了左边最大值，右指针不断向左逼近就行，此方法为O(n) 时间, O(1) 空间，第四种方法类似第三种，左右夹逼，每次选择小的那个指针向中间靠拢，此方法为O(n) 时间, O(1) 空间，但是要比方法三少遍历一遍。

# 完整代码
```python
# 方法三
class Solution:
    # @param heights: a list of integers
    # @return: a integer
    def trapRainWater(self, heights):
        water = 0
        MaxIndex = -1
        MaxHeight = -1
        for i in range(1, len(heights)):
            if heights[i] > MaxHeight:
                MaxHeight = heights[i]
                MaxIndex = i
                
        prev = 0
        for i in range(0, MaxIndex):
            if heights[i] > prev:
                water += (heights[i] - prev) * (MaxIndex - i)
                prev = heights[i]
            water -= heights[i]
            
        prev = 0
        for i in range(len(heights) - 1, MaxIndex, -1):
            if heights[i] > prev:
                water += (heights[i] - prev) * (i - MaxIndex)
                prev = heights[i]
            water -= heights[i]
            
        return water

# 方法四
class Solution:
    # @param heights: a list of integers
    # @return: a integer
    def trapRainWater(self, heights):
        n = len(heights)
        l, r, water, minHeight = 0, n - 1, 0, 0
        while l < r:
            while l < r and heights[l] <= minHeight:
                water += minHeight - heights[l]
                l += 1
            while l < r and heights[r] <= minHeight:
                water += minHeight - heights[r]
                r -= 1
            minHeight = min(heights[l], heights[r])
        return water
```