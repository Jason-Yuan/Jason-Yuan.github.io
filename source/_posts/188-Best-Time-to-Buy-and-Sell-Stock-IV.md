title: "188 Best Time to Buy and Sell Stock IV"
date: 2015-10-04 17:02:05
tags: ["LeetCode", "Array", "Dynamic Programing"]
categories: "LeetCode"
---

# 原题
>假设你有一个数组，它的第i个元素是一支给定的股票在第i天的价格。
设计一个算法来找到最大的利润。你最多可以完成 k 笔交易。

给定价格 = [4,4,6,1,1,4,2,5], 且 k = 2, 返回 6.

你不可以同时参与多笔交易(你必须在再次购买前出售掉之前的股票)

# 解题思路
* 动态规划，**循环引用的状态数组(local/global数组)**
* mustSell[i][k] 表示前i天，至多进行k次交易，第i天必须sell的最大获益
* globalbest[i][k] 表示前i天，至多进行k次交易，第i天可以不sell的最大获益
* 状态转移方程
```python
mustSell[i][k] = max(globalBest[i - 1][k - 1] + profit, mustSell[i - 1][k] + profit)
globalBest[i][k] = max(globalBest[i - 1][k], mustSell[i][k])
```

# 完整代码
```python
class Solution(object):
    def maxProfit(self, k, prices):
        """
        :type k: int
        :type prices: List[int]
        :rtype: int
        """
        if k == 0:
            return 0
            
        if k >= len(prices) / 2:
            profit = 0
            for i in range(1, len(prices)):
                diff = prices[i] - prices[i - 1]
                if diff > 0:
                    profit += diff
            return profit
            
        n = len(prices)
        mustSell = [[0 for i in range(k + 1)] for i in range(n + 1)]
        globalBest = [[0 for i in range(k + 1)] for i in range(n + 1)]
        
        for k in range(k + 1):
            mustSell[0][k] = globalBest[0][k] = 0
            
        for i in range(1, n):
            profit = prices[i] - prices[i - 1]
            mustSell[i][0] = 0
            for k in range(1, k + 1):
                mustSell[i][k] = max(globalBest[i - 1][k - 1] + profit, mustSell[i - 1][k] + profit)
                globalBest[i][k] = max(globalBest[i - 1][k], mustSell[i][k])
                
        return globalBest[n - 1][k]
```