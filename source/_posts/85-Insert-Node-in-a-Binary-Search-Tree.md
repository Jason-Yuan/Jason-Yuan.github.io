title: "85 Insert Node in a Binary Search Tree"
date: 2015-10-22 02:35:26
tags: ["LintCode", "BST", "Recursion"]
categories: "LintCode"
---

# 原题
>给定一棵二叉查找树和一个新的树节点，将节点插入到树中。
你需要保证该树仍然是一棵二叉查找树。

给出如下一棵二叉查找树，在插入节点6之后这棵二叉查找树可以是这样的：
```
  2             2
 / \           / \
1   4   -->   1   4
   /             / \ 
  3             3   6
```

# 解题思路
* 如果root为空，给root赋值
* root不为空，根据root.val与node.val的大小去判断往左儿子走还是右儿子走

# 完整代码
```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
class Solution:
    """
    @param root: The root of the binary search tree.
    @param node: insert this node into the binary search tree.
    @return: The root of the new binary search tree.
    """
    def insertNode(self, root, node):
        if root is None:
            root = node
        elif root.val > node.val:
            if root.left:
                self.insertNode(root.left, node)
            else:
                root.left = node
        else:
            if root.right:
                self.insertNode(root.right, node)
            else:
                root.right = node
        return root
```