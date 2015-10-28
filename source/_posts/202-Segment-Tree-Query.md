title: "202 Segment Tree Query"
date: 2015-10-05 00:11:03
tags: ["LintCode", "Segment Tree", "Divide and Conquer"]
categories: "LintCode"
---

# 原题
>对于一个有n个数的整数数组，在对应的线段树中, 根节点所代表的区间为0-n-1, 每个节点有一个额外的属性max，值为该节点所代表的数组区间start到end内的最大值。
为SegmentTree设计一个query的方法，接受3个参数root, start和end，线段树root所代表的数组中子区间[start, end]内的最大值。

对于数组 [1, 4, 2, 3], 对应的线段树为：
```
                  [0, 3, max=4]
                 /             \
          [0,1,max=4]        [2,3,max=3]
          /         \        /         \
   [0,0,max=1] [1,1,max=4] [2,2,max=2], [3,3,max=3]
```
query(root, 1, 1), return 4
query(root, 1, 2), return 4
query(root, 2, 3), return 3
query(root, 0, 2), return 4

# 解题思路
* 本题为**下标型线段树**的Query
* 如果查询区间 == 节点表示区间 => 直接返回root.max
* 如果查询区间被节点表示的左/右区间包含 => 递归搜索左/右区间
* 如果查询区间和结点表示的区间相交不相等 => 分裂递归搜索左/右区间
* 典型的divide & conquer
* 最后返回max(leftMax, rightMax)

# 完整代码
```python
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end, max):
        self.start, self.end, self.max = start, end, max
        self.left, self.right = None, None
"""

class Solution:	
    # @param root, start, end: The root of segment tree and 
    #                          an segment / interval
    # @return: The maximum number in the interval [start, end]
    def query(self, root, start, end):
        if start > end or root == None:
            return - sys.maxint
            
        if root.start >= start and root.end <= end:
            return root.max
            
        mid = root.start + (root.end - root.start) / 2
        leftMax, rightMax = - sys.maxint - 1, - sys.maxint - 1
        if start <= mid:
            if mid < end:
                leftMax = self.query(root.left, start, mid)
            else:
                leftMax = self.query(root.left, start, end)
        if mid < end:
            if start <= mid:
                rightMax = self.query(root.right, mid + 1, end)
            else:
                rightMax = self.query(root.right, start, end)
        return max(leftMax, rightMax)
```