title: "203 Segment Tree Modify"
date: 2015-10-05 02:09:54
tags: ["LintCode", "Segment Tree", "Recursion"]
categories: "LintCode"
---

# 原题
>对于一棵 最大线段树, 每个节点包含一个额外的 max 属性，用于存储该节点所代表区间的最大值。
设计一个 modify 的方法，接受三个参数 root、 index 和 value。该方法将 root 为跟的线段树中 [start, end] = [index, index] 的节点修改为了新的 value ，并确保在修改后，线段树的每个节点的 max 属性仍然具有正确的值。

对于线段树:
```python
                      [1, 4, max=3]
                    /                \
        [1, 2, max=2]                [3, 4, max=3]
       /              \             /             \
[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]
```
如果调用 modify(root, 2, 4), 返回:
```python
                      [1, 4, max=4]
                    /                \
        [1, 2, max=4]                [3, 4, max=3]
       /              \             /             \
[1, 1, max=2], [2, 2, max=4], [3, 3, max=0], [4, 4, max=3]
```
或 调用 modify(root, 4, 0), 返回:
```python
                     [1, 4, max=2]
                    /                \
        [1, 2, max=2]                [3, 4, max=0]
       /              \             /             \
[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=0]
```

# 解题思路
* 自上而下，递归查找
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
    """
    @param root, index, value: The root of segment tree and 
    @ change the node's value with [index, index] to the new given value
    @return: nothing
    """
    def modify(self, root, index, value):
        if root.start == index and root.end == index:
            root.max = value
            return
            
        mid = (root.start + root.end) / 2
        if root.start <= index and index <= mid:
            self.modify(root.left, index, value)
        elif mid < index and index <= root.end:
            self.modify(root.right, index, value)
        root.max = max(root.left.max, root.right.max)
```