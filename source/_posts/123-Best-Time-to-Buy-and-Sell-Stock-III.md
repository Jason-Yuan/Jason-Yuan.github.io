title: "123 Best Time to Buy and Sell Stock III"
date: 2015-09-29 00:23:54
tags: ["LeetCode", "Array", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>假设你有一个数组，它的第i个元素是一支给定的股票在第i天的价格。设计一个算法来找到最大的利润。你最多可以完成两笔交易。

给出一个样例数组 [4,4,6,1,1,4,2,5], 返回 6

# 解题思路
* 类似题[Maximum Subarray II]
* 遍历数组，以每一个数作为分割线分别求左右两边各一次交易最大获利，即[Best Time to Buy and Sell Stock I]
* 具体实现，两次分别从左向右和从右向左遍历数组，DP求解
* 最后遍历一遍，左右最大相加，不断更新维护profit

# 完整代码
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if not prices or len(prices) == 1:
            return 0
            
        size = len(prices)
        left, right = [0 for i in range(size)], [0 for i in range(size)]
        # DP from left to right;
        left[0] = 0
        minPrice = prices[0]
        for i in range(size):
            minPrice = min(prices[i], minPrice)
            left[i] = max(left[i - 1], prices[i] - minPrice)
        
        # DP from right to left;
        right[size - 1] = 0
        maxPrice = prices[size - 1]
        for i in range(size - 2, -1, -1):
            maxPrice = max(prices[i], maxPrice)
            right[i] = max(right[i + 1], maxPrice - prices[i])
        
        profit = -sys.maxint - 1
        for i in range(size):
            profit = max(left[i] + right[i], profit)
            
        return profit
```