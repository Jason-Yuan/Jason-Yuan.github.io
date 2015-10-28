title: "235 Lowest Common Ancestor of a Binary Search Tree"
date: 2015-10-22 01:09:04
tags: ["LeetCode", "BST", "LCA", "Recursion"]
categories: "LeetCode"
---

# 原题
>给定一棵平衡二叉树(BST)，找到两个节点的最近公共父节点(LCA)。
最近公共祖先是两个节点的公共的祖先节点且具有最大深度。

对于下面这棵二叉树
```
  4
 / \
3   7
   / \
  5   6
```
LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7

# 解题思路
* 与本题类似的题是把BST换成普通的二叉树，方法为divide & conquer
* 对于本题，初始如果`root`为空，则直接返回None
* 如果有其中一个`node`跟`root`相等，则返回`root`
* 以上两两种情况都不满足，考虑到是一棵平衡二叉树，所以有以下三种情况
 * 如果两个`node`中的较小值都大于`root.val`，则答案一定在右子树
 * 如果两个`node`中的较大值都小于`root.val`，则答案一定在左子树
 * 或者`root.val`的值介于两个`node`的值中间，则返回`root`

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root:
            return None
        if root == p or root == q:
            return root.val

        if q.val > p.val:
            q, p = p, q
        if q.val < root.val and p.val > root.val:
            return root.val
        if q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        if p.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
```