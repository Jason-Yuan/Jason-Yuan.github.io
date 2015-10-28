title: "145 Binary Tree Postorder Traversal"
date: 2015-10-22 02:42:32
tags: ["LeetCode", "BST", "Recursion"]
categories: "LeetCode"
---

# 原题
>给出一棵二叉树，返回其节点值的后序遍历。

给出一棵二叉树 {1,#,2,3}
```
   1
    \
     2
    /
   3
```
返回 [3,2,1]

# 解题思路
* 递归求解，定义一个helper函数，定义一个result全局变量，传入helper函数

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
    @param root: The root of binary tree.
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        res = []
        self.helper(root, res)
        return res
            
    def helper(self, root, result):
        if root:
            self.helper(root.left, result)
            self.helper(root.right, result)
            result.append(root.val)
```