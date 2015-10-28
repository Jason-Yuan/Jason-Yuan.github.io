title: "122 Best Time to Buy and Sell Stock II"
date: 2015-10-04 02:42:01
tags: ["LeetCode", "Array", "Greedy"]
categories: "LeetCode"
---

# 原题
>假设有一个数组，它的第i个元素是一个给定的股票在第i天的价格。设计一个算法来找到最大的利润。你可以完成尽可能多的交易(多次买卖股票)。然而,你不能同时参与多个交易(你必须在再次购买前出售股票)。

给出一个数组样例[2,1,2,0,1], 返回 2

# 解题思路
* 在`Best Time to Buy and Sell Stock`系列中，和本题最像的是`Best Time to Buy and Sell Stock IV`
* `Best Time to Buy and Sell Stock IV`中是求某个给定k次交易的最大收益，和`Maximum Subarray III`完全一样
* 本题由于是可以操作任意次数，只为获得最大收益，而且对于一个上升子序列，比如：[5, 1, 2, 3, 4]中的1, 2, 3, 4序列来说，对于两种操作方案：
1 在1买入，4卖出
2 在1买入，2卖出同时买入，3卖出同时买入，4卖出
这两种操作下，收益是一样的。
* 所以可以从头到尾扫描prices，如果`price[i] – price[i-1]`大于零则计入最后的收益中。即**贪心法**

# 完整代码
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        profit = 0
        for i in range(1, len(prices)):
            diff = prices[i] - prices[i - 1]
            if diff > 0:
                profit += diff
        return profit
```