title: "207 Interval Sum II"
date: 2015-10-06 01:05:37
tags: ["LintCode", "Array", "Segment Tree"]
categories: "LintCode"
---

# 原题
>在类的构造函数中给一个整数数组, 实现两个方法 query(start, end) 和 modify(index, value):
对于 query(start, end), 返回数组中下标 start 到 end 的和。
对于 modify(index, value), 修改数组中下标为 index 上的数为 value.

给定数组 A = [1,2,7,8,5].
query(0, 2), 返回 10.
modify(0, 4), 将 A[0] 修改为 4.
query(0, 1), 返回 6.
modify(2, 1), 将 A[2] 修改为 1.
query(2, 4), 返回 14.

# 解题思路
* 同[Interval Sum I]的思路一样，只是做成了Interval Sum类，实现API
* 分别定义了`build`, `queryHelper `, `modifyHelper `三个helper函数，相应的API接口调用helper函数

# 完整代码
```python
class segmentTreeNode:
    def __init__(self, start, end, Sum):
        self.start, self.end, self.Sum = start, end, Sum
        self.left, self.right = None, None
        
class Solution:	
    
    # @param A: An integer list
    def __init__(self, A):
        self.root = self.build(A, 0, len(A) - 1)
    
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

    # @param start, end: Indices
    # @return: The sum from start to end
    def query(self, start, end):
        return self.queryHelper(self.root, start, end)
    
    def queryHelper(self, root, start, end):
        if root.start == start and root.end == end:
            return root.Sum
            
        mid = root.start + (root.end - root.start) / 2
        leftSum, rightSum = 0, 0
        if start <= mid:
            if mid < end:
                leftSum = self.queryHelper(root.left, start, mid)
            else:
                leftSum = self.queryHelper(root.left, start, end)
        if mid < end:
            if start <= mid:
                rightSum = self.queryHelper(root.right, mid + 1, end)
            else:
                rightSum = self.queryHelper(root.right, start, end)
        return leftSum + rightSum

    # @param index, value: modify A[index] to value.
    def modify(self, index, value):
        self.modifyHelper(self.root, index, value)
        
    def modifyHelper(self, root, index, value):
        if root.start == index and root.end == index:
            root.Sum = value
            return
            
        mid = root.start + (root.end - root.start) / 2
        if root.start <= index and index <= mid:
            self.modifyHelper(root.left, index, value)
        if mid < index and index <= root.end:
            self.modifyHelper(root.right, index, value)
        root.Sum =  root.left.Sum + root.right.Sum
```