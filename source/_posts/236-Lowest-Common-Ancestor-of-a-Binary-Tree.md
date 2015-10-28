title: "236 Lowest Common Ancestor of a Binary Tree"
date: 2015-10-22 01:18:10
tags: ["LeetCode", "Binary Tree", "LCA", "Divide and Conquer"]
categories: "LeetCode"
---

# 原题
>给定一棵二叉树，找到两个节点的最近公共父节点(LCA)。
最近公共祖先是两个节点的公共的祖先节点且具有最大深度。

对于下面这棵二叉树
```
  4
 / \
3   7
   / \
  5   6
LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7
```

# 解题思路
* Divide & Conquer 的思路
* 如果`root`为空，则返回空
* 如果`root`等于其中某个`node`，则返回`root`
* 如果上述两种情况都不满足，则divide，左右子树分别调用该方法
* Divide & Conquer中**治**这一步要考虑清楚，本题三种情况
 * 如果`left`和`right`都有结果返回，说明root是最小公共祖先
 * 如果只有`left`有返回值，说明`left`**的返回值**是最小公共祖先
 * 如果只有`right`有返回值，说明`right`**的返回值**是最小公共祖先

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
            return root
        
        # divide
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        
        # conquer
        if left != None and right != None:
            return root
        if left != None:
            return left
        else:
            return right
```