title: "121 Best Time to Buy and Sell Stock I"
date: 2015-09-28 21:09:39
tags: ["LeetCode", "Array", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>假设有一个数组，它的第i个元素是一支给定的股票在第i天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。

给出一个数组样例[3,2,3,1,2], 返回 1 

# 解题思路
* 相似题[Maximum Subarray]，本题是找两个点使得end-start最大，[Maximum Subarray]是找一个区间使得区间内值的和最大
* 想求y-x的最大值，则让y尽可能大，让x尽可能小

# 完整代码
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if not prices:
            return 0
        
        res = - sys.maxint
        end = 0
        minStart = prices[0]
        for i in range(1, len(prices)):
            end = prices[i]
            res = max(res, end - minStart)
            minStart = min(minStart, end)

        return 0 if res < 0 else res
```