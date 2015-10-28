title: "249 Count the Smaller Number Before Itself"
date: 2015-10-05 21:56:10
tags: ["LintCode", "Array", "Segment Tree"]
categories: "LintCode"
---

# 原题
>给定一个整数数组（下标由 0 到 n-1， n 表示数组的规模，取值范围由 0 到10000）。对于数组中的每个 ai 元素，请计算 ai 前的数中比它小的元素的数量。

对于数组[1,2,7,8,5] ，返回 [0,1,2,3,2]

# 解题思路
* 建立**值型线段树**
* 首先建立树count都等于0，然后循环query再modify
* 注意查找x-1可能出现负值，要做判断，x-1 < 0直接`res.append(0)`

# 完整代码
```python
class segmentTreeNode:
    def __init__(self, start, end, count):
        self.start, self.end, self.count = start, end, count
        self.left, self.right = None, None
  
class Solution:
    """
    @param A: A list of integer
    @return: Count the number of element before this element 'ai' is 
             smaller than it and return count number list
    """
    def build(self, start, end):
        if start == end:
            root = segmentTreeNode(start, end, 0)
            return root
            
        root = segmentTreeNode(start, end, 0)
        if start != end:
            mid = (start + end) / 2
            root.left = self.build(start, mid)
            root.right = self.build(mid + 1, end)
        return root
        
    def query(self, root, start, end):
        if root.start == start and root.end == end:
            return root.count
            
        mid = root.start + (root.end - root.start) / 2
        leftCount, rightCount = 0, 0
        if start <= mid:
            if mid < end:
                leftCount = self.query(root.left, start, mid)
            else:
                leftCount = self.query(root.left, start, end)
        if mid < end:
            if start <= mid:
                rightCount = self.query(root.right, mid + 1, end)
            else:
                rightCount = self.query(root.right, start, end)
        return leftCount + rightCount
        
    def modify(self, root, index, value):
        if root.start == index and root.end == index:
            root.count += value
            return
        
        mid = root.start + (root.end - root.start) / 2
        if root.start <= index and index <= mid:
            self.modify(root.left, index, value)
        
        if mid < index and index <= root.end:
            self.modify(root.right, index, value)
        
        root.count = root.left.count + root.right.count
        
    def countOfSmallerNumberII(self, A):
        root = self.build(0, 10000)
        
        res = []
        for num in A:
            ans = 0
            if num > 0:
                ans = self.query(root, 0, num - 1)
            self.modify(root, num, 1)
            res.append(ans)
        
        return res
```