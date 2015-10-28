title: "206 Interval Sum I"
date: 2015-10-06 00:50:23
tags: ["LintCode", "Array", "Segment Tree"]
categories: "LintCode"
---

# 原题
>给定一个整数数组（下标由 0 到 n-1，其中 n 表示数组的规模），以及一个查询列表。每一个查询列表有两个整数 [start, end] 。 对于每个查询，计算出数组中从下标 start 到 end 之间的数的总和，并返回在结果列表中。

对于数组 [1,2,7,8,5]，查询[(1,2),(0,4),(2,4)], 返回 [9,23,20]

# 解题思路
* 建立**下标型线段树**， 自下而上，回溯更新
* `root.sum = root.left.sum + root.right.sum`
* for循环query更新结果

# 完整代码
```python
"""
Definition of Interval.
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""
class segmentTreeNode:
    def __init__(self, start, end, Sum):
        self.start, self.end, self.Sum = start, end, Sum
        self.left, self.right = None, None

class Solution:	
    """
    @param A, queries: Given an integer array and an Interval list
                       The ith query is [queries[i-1].start, queries[i-1].end]
    @return: The result list
    """
    def build(self, L, start, end):
        if start > end:
            return None
            
        root = segmentTreeNode(start, end, 0)
        if start != end:
            mid = (start + end) / 2
            root.left = self.build(L, start, mid)
            root.right = self.build(L, mid + 1, end)
            root.Sum = root.left.Sum + root.right.Sum
        else:
            root.Sum = L[start]
        return root
        
    def query(self, root, start, end):
        if root.start == start and root.end == end:
            return root.Sum
            
        mid = root.start + (root.end - root.start) / 2
        leftSum, rightSum = 0, 0
        if start <= mid:
            if mid < end:
                leftSum = self.query(root.left, start, mid)
            else:
                leftSum = self.query(root.left, start, end)
        if mid < end:
            if start <= mid:
                rightSum = self.query(root.right, mid + 1, end)
            else:
                rightSum = self.query(root.right, start, end)
        return leftSum + rightSum
        
    def intervalSum(self, A, queries):
        root = self.build(A, 0, len(A) - 1)
        res = []
        for q in queries:
            res.append(self.query(root, q.start, q.end))
        return res
```