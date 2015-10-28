title: "205 Interval Minimum Number"
date: 2015-10-05 02:33:46
tags: ["LintCode", "Array", "Segment Tree"]
categories: "LintCode"
---

# 原题
>给定一个整数数组（下标由 0 到 n-1，其中 n 表示数组的规模），以及一个查询列表。每一个查询列表有两个整数 [start, end]。 对于每个查询，计算出数组中从下标 start 到 end 之间的数的最小值，并返回在结果列表中。

对于数组 [1,2,7,8,5]， 查询 [(1,2),(0,4),(2,4)]，返回 [2,1,5]

# 解题思路
* 使用线段树，典型例题
* 首先构造node类，实现Query和Build

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
    def __init__(self, start, end, min):
        self.start, self.end, self.min = start, end, min
        self.left, self.right = None, None
        
class Solution:	
    """
    @param A, queries: Given an integer array and an Interval list
                       The ith query is [queries[i-1].start, queries[i-1].end]
    @return: The result list
    """
    def build(self, A):
        if not A:
            return None
        
        return self.helper(A, 0, len(A) - 1)

    def helper(self, L, start, end):
        if start == end:
            root = segmentTreeNode(start, end, L[start])
            return root
            
        root = segmentTreeNode(start, end, 0)
        if start != end:
            mid = (start + end) / 2
            root.left = self.helper(L, start, mid)
            root.right = self.helper(L, mid + 1, end)
        root.min = min(root.left.min, root.right.min)
        return root
        
    def query(self, root, start, end):
        if start > end or root == None:
            return sys.maxint
            
        if root.start >= start and root.end <= end:
            return root.min
            
        mid = root.start + (root.end - root.start) / 2
        leftMin, rightMin = sys.maxint, sys.maxint
        if start <= mid:
            if mid < end:
                leftMin = self.query(root.left, start, mid)
            else:
                leftMin = self.query(root.left, start, end)
        if mid < end:
            if start <= mid:
                rightMin = self.query(root.right, mid + 1, end)
            else:
                rightMin = self.query(root.right, start, end)
        return min(leftMin, rightMin)
        
    def intervalMinNumber(self, A, queries):
        # write your code here
        root = self.build(A)
        res = []
        for q in queries:
            res.append(self.query(root, q.start, q.end))
        return res
```