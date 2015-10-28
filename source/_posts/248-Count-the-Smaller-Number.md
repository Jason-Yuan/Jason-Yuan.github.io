title: "248 Count the Smaller Number"
date: 2015-10-05 21:51:06
tags: ["LintCode", "Array", "Binary Search", "Quick Sort", "Segment Tree"]
categories: "LintCode"
---

# 原题
>给定一个整数数组 （下标由 0 到 n-1，其中 n 表示数组的规模，数值范围由 0 到 10000），以及一个 查询列表。对于每一个查询，将会给你一个整数，请你返回该数组中小于给定整数的元素的数量。

对于数组 [1,2,7,8,5] ，查询 [1,8,5]，返回 [0,4,2]

# 解题思路
* 方法一，将数组快速排序，然后对于某个数x，二分查找最后一个x-1的位置
* 方法二，建立**值型线段树**
* 对于方法二要注意查找x-1可能出现负值，要做判断，x-1 < 0直接`res.append(0)`

# 完整代码
```python
# 方法一
class Solution:
    """
    @param A: A list of integer
    @return: The number of element in the array that 
             are smaller that the given integer
    """
    def countOfSmallerNumber(self, A, queries):
        A.sort()
        res = []
        for q in queries:
            res.append(self.SmallerNumber(A, q))
        return res
        
    def SmallerNumber(self, L, num):
        if len(L) == 0 or L[-1] < num:
            return len(L)
            
        start, end = 0, len(L) - 1
        while start + 1 < end:
            mid = start + (end - start) / 2
            if L[mid] >= num:
                end = mid
            else:
                start = mid
        
        if L[start] >= num:
            return start
        if L[end] >= num:
            return end

# 方法二
class segmentTreeNode:
    def __init__(self, start, end, count):
        self.start, self.end, self.count = start, end, count
        self.left, self.right = None, None
        
class Solution:
    """
    @param A: A list of integer
    @return: The number of element in the array that 
             are smaller that the given integer
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
        
    def countOfSmallerNumber(self, A, queries):
        root = self.build(0, 1000)
        
        for num in A:
            self.modify(root, num, 1)
            
        res = []
        for q in queries:
            ans = 0
            if q > 0:
                ans = self.query(root, 0, q - 1)
            res.append(ans)
        
        return res
```