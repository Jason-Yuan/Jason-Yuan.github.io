title: "84 Largest Rectangle in Histogram"
date: 2015-09-27 23:35:05
tags: ["LeetCode", "Stack", "Ascending order stack"]
categories: "LeetCode"
---

# 原题
>给出的n个非负整数表示每个直方图的高度，每个直方图的宽均为1，在直方图中找到最大的矩形面积。

给出 height = **[2,1,5,6,2,3]**，返回 **10** (5 * 2)

# 解题思路
* DP一般是把n!/n^n/2^n优化到n^2/n^3，而本题暴力解法是n^2，我们希望优化到n，不是DP的强项
* n^2 -> n （stack强项）
* 求出把每个柱子当做最小的得到的面积 -> 求出n个值，取min
* 通过维护一个**升序stack**，实现 -> 找到每个数左边第一个比它小的和右边第一个比它小的
```python
# push 2
stack = [] -> [2]
# push 1 ,以为2 > 1，所以pop 2以维持升序stack
# 2左边第一个比它小的就是none，右边第一个比它小的就是1
stack = [2] -> [1]
```
* Conditional Operator in Python
```python
# Java version
a > 10 ? b : c
# Python version
b if a > 10 else c
```

# 完整代码
```python
class Solution(object):
    def largestRectangleArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        if not height:
            return 0
            
        res = 0
        stack = []
        height.append(-1)
        for i in range(len(height)):
            current = height[i]
            while len(stack) != 0 and current <= height[stack[-1]]:
                h = height[stack.pop()]
                w = i if len(stack) == 0 else i - stack[-1] - 1
                res = max(res, h * w)
            stack.append(i)
        return res
```