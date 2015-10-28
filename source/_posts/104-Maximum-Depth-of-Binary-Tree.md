title: "104 Maximum Depth of Binary Tree"
date: 2015-10-20 23:40:31
tags: ["LeetCode", "BST", "Divide and Conquer", "Recursion"]
categories: "LeetCode"
---

# 原题
>给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的距离。

给出一棵如下的二叉树
```
  1
 / \ 
2   3
   / \
  4   5
```
这个二叉树的最大深度为3.

# 解题思路
* Divide & Conquer
* 整个树的树高取决于左子树与右子树的树高中的最大值+1
* 整个问题被划分为求左子树树高和右子树树高

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```