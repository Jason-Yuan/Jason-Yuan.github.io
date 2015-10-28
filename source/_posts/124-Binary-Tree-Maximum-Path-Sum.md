title: "124 Binary Tree Maximum Path Sum"
date: 2015-10-21 00:48:42
tags: ["LeetCode", "Binary Tree", "Recursion", "Divide and Conquer"]
categories: "LeetCode"
---

# 原题
>给出一棵二叉树，寻找一条路径使其路径和最大，路径可以在任一节点中开始和结束（路径和为两个节点之间所在路径上的节点权值之和）

给出一棵二叉树：
```
       1
      / \
     2   3
```
返回 6

# 解题思路
* 首先，想一个简化版(single path)，找从`root`到任意点得最大值。类似于maxDepth，每次加`root.val`而不再是`+1`
* 求单路的时候，如果`root`加左儿子单路或者右儿子单路最后的值都小于0，则返回0，意味着不要`root`开始的这个单路了
* 本题思路，divide & conquer
求最大路径和就等于下面三个值的最大值：
 * 左子树的最大路径和
 * 右子树最大路径和
 * 左子树单路 + 右子树单路 + root.val

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxPathSum(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        res, _ = self.helper(root)
        return res
        
    def helper(self, root):
        if not root:
            return -sys.maxint, 0
            
        left = self.helper(root.left)
        right = self.helper(root.right)
        singlePathSum = max(left[1] + root.val, right[1] + root.val, 0)
        maxPathSum = max(left[0], right[0], left[1] + right[1] + root.val)
        return maxPathSum, singlePathSum
```