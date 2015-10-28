title: "70 Climbing Stairs"
date: 2015-10-25 03:20:40
tags: ["LeetCode", "Sequence DP", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？

比如n=3，1+1+1=1+2=2+1=3，共有3中不同的方法
返回 3

# 解题思路
* 本题本质和菲波那切数列问题非常相似
* 单序列型动态规划问题 - Sequence DP
* cache[i]表示有i个台阶，能跳到第i个台阶的方案个数
* 因为每次可以跳一步或者跳两步，所以cache[i]就等于下面的和
 * 可以跳的i - 1的方案个数即cache[i - 1]
 * 可以跳到i - 2的方案个数即cache[i - 2]
 

# 完整代码
```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n < 1:
            return 0
        if n == 1:
            return 1
        if n == 2:
            return 2
        cache = [0 for i in range(n)]
        cache[0] = 1
        cache[1] = 2
        for i in range(2, n):
            cache[i] = cache[i - 1] + cache[i - 2]
        return cache[n - 1]
```