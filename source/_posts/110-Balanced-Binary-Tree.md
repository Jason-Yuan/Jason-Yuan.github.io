title: "110 Balanced Binary Tree"
date: 2015-10-20 23:57:39
tags: ["LeetCode", "BST", "Divide and Conquer", "Recursion"]
categories: "LeetCode"
---

# 原题
>给定一个二叉树,确定它是高度平衡的。对于这个问题,一棵高度平衡的二叉树的定义是：一棵二叉树中每个节点的两个子树的深度相差不会超过1。 

给出二叉树 A={3,9,20,#,#,15,7}, B={3,#,20,15,7}
```
A)  3            B)    3 
   / \                  \
  9  20                 20
    /  \                / \
   15   7              15  7
```
二叉树A是高度平衡的二叉树，但是B不是

# 解题思路
* 一般BST的问题，首先往divide & conquer的思路去想
* 一棵二叉树是不是平衡二叉树 => 左子树是不是平衡二叉树？右子树是不是平衡二叉树？
 * 如果任意一颗子树不平衡，整个树肯定不平衡
 * 如果左右子树都是平衡的，但左右子树相差高度大于1，此刻这课树又是不平衡的
 * 问题转化为分别对左右子树求maxDepth，平衡就返回最大深度，不平衡就返回-1
* 技巧：用-1表示它不是一颗平衡二叉树

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        return self.maxDepth(root) != -1
        
    def maxDepth(self, root):
        if not root:
            return 0
        
        left = self.maxDepth(root.left)
        right = self.maxDepth(root.right)
        if left == -1 or right == -1 or abs(left - right) > 1:
            return -1
        else:
            return max(left, right) + 1
```