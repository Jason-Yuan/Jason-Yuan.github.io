title: "247 Segment Tree Query II"
date: 2015-10-05 00:16:09
tags: ["LintCode", "Segment Tree", "Divide and Conquer"]
categories: "LintCode"
---

# 原题
>对于一个数组，我们可以对其建立一棵线段树, 每个结点存储一个额外的值count来代表这个结点所指代的数组区间内的元素个数. (数组中并不一定每个位置上都有元素)
实现一个query的方法，该方法接受三个参数root, start和end, 分别代表线段树的根节点和需要查询的区间，找到数组中在区间[start, end]内的元素个数。

对于数组 [0, 2, 3], 对应的线段树为：
```
                     [0, 3, count=3]
                     /             \
          [0,1,count=1]             [2,3,count=2]
          /         \               /            \
   [0,0,count=1] [1,1,count=0] [2,2,count=1], [3,3,count=1]
```
query(1, 1), return 0
query(1, 2), return 1
query(2, 3), return 2
query(0, 2), return 2

# 解题思路
* 本题为**值型线段树**的Query
* 如果查询区间 == 节点表示区间 => 直接返回root.count
* 如果查询区间被节点表示的左/右区间包含 => 递归搜索左/右区间
* 如果查询区间和结点表示的区间相交不相等 => 分裂递归搜索左/右区间
* 典型的divide & conquer
* 最后返回 leftCount + rightCount

# 完整代码
```python
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end, count):
        self.start, self.end, self.count = start, end, count
        self.left, self.right = None, None
"""

class Solution:	
    # @param root, start, end: The root of segment tree and 
    #                          an segment / interval
    # @return: The count number in the interval [start, end] 
    def query(self, root, start, end):
        if start > end or root == None:
            return 0
            
        if root.start >= start and root.end <= end:
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
```