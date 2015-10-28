title: "439 Segment Tree Build II"
date: 2015-10-05 01:45:57
tags: ["LintCode", "Segment Tree", "Recursion", "Divide and Conquer"]
categories: "LintCode"
---

# 原题
>线段树是一棵二叉树，他的每个节点包含了两个额外的属性start和end用于表示该节点所代表的区间。start和end都是整数，并按照如下的方式赋值:
根节点的 start 和 end 由 build 方法所给出。
对于节点 A 的左儿子，有 start=A.left, end=(A.left + A.right) / 2。
对于节点 A 的右儿子，有 start=(A.left + A.right) / 2 + 1, end=A.right。
如果 start 等于 end, 那么该节点是叶子节点，不再有左右儿子。
实现一个 build 方法，接受 start 和 end 作为参数, 然后构造一个代表区间 [start, end] 的线段树，每个节点包含这段区间的最大值，返回这棵线段树的根。

给定[3,2,1,4]. 对应的线段树为：
```python
                 [0,  3] (max = 4)
                  /            \
        [0,  1] (max = 3)     [2, 3]  (max = 4)
        /        \               /             \
[0, 0](max = 3)  [1, 1](max = 2)[2, 2](max = 1) [3, 3] (max = 4)
```

# 解题思路
* 自上而下，递归分裂
* 自下而上，回溯更新

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
    # @oaram A: a list of integer
    # @return: The root of Segment Tree
    def build(self, A):
        if not A:
            return None
        
        return self.helper(A, 0, len(A) - 1)

    def helper(self, L, start, end):
        if start == end:
            root = SegmentTreeNode(start, end, L[start])
            return root
            
        root = SegmentTreeNode(start, end, 0)
        if start != end:
            mid = (start + end) / 2
            root.left = self.helper(L, start, mid)
            root.right = self.helper(L, mid + 1, end)
        root.max = max(root.left.max, root.right.max)
        return root
```