title: "11 Search Range In Binary Search Tree"
date: 2015-10-21 03:14:34
tags: ["LintCode", "BST", "DFS", "Recursion"]
categories: "LintCode"
---

# 原题
>给定两个值 k1 和 k2（k1 < k2）和一个二叉查找树的根节点。找到树中所有值在 k1 到 k2 范围内的节点。即打印所有x (k1 <= x <= k2) 其中 x 是二叉查找树的中的节点值。返回所有升序的节点值。

如果有 k1 = 10 和 k2 = 22, 你的程序应该返回 [12, 20, 22].
```
    20
   /  \
  8   22
 / \
4   12
```

# 解题思路
* 方法一：暴力解法，DFS遍历所有节点，符合要求的加入result中，最后result排序，返回
* 方法二：同样是DFS遍历，但加入判断，相当于剪枝。
 * 如果`root.val > start`则可以继续左子树
 * 如果`root.val < end`则可以继续右子树
 * 如果`start <= root.val <= end`则把`root.val`加入`result`

# 完整代码
```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

# 方法一
class Solution:
    """
    @param root: The root of the binary search tree.
    @param k1 and k2: range k1 to k2.
    @return: Return all keys that k1<=key<=k2 in ascending order.
    """     
    def searchRange(self, root, k1, k2):
        res = []
        self.dfs(root, k1, k2, res)
        return sorted(res)
        
    def dfs(self, root, start, end, result):
        if root != None:
            if root.val >= start and root.val <= end:
                result.append(root.val)
            self.dfs(root.left, start, end, result)
            self.dfs(root.right, start, end, result)

# 方法二
class Solution:
    """
    @param root: The root of the binary search tree.
    @param k1 and k2: range k1 to k2.
    @return: Return all keys that k1<=key<=k2 in ascending order.
    """     
    def searchRange(self, root, k1, k2):
        res = []
        self.dfs(root, k1, k2, res)
        return sorted(res)
        
    def dfs(self, root, start, end, result):
        if root != None:
            if root.val > start:
                self.dfs(root.left, start, end, result)
            if root.val >= start and root.val <= end:
                result.append(root.val)
            if root.val < end:
                self.dfs(root.right, start, end, result)
```