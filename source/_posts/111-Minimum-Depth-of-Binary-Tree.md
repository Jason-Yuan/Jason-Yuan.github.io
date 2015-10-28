title: "111 Minimum Depth of Binary Tree"
date: 2015-10-22 01:40:09
tags: ["LeetCode", "Binary Tree", "Divide and Conquer"]
categories: "LeetCode"
---

# 原题
>给定一个二叉树，找出其最小深度。
二叉树的最小深度为根节点到最近叶子节点的距离。

给出一棵如下的二叉树:
```
        1
     /     \ 
   2       3
          /    \
        4      5  
```
这个二叉树的最小深度为 2

# 解题思路
* **一定要注意定义**，比如下面这棵树的最小深度是**3**不是**1**
```
   1
    \
     2
      \
       3
```
* 方法和找最大深度一样，Divide & Conquer
只需要判断如果左右儿子中又None，则返回非None的儿子的高度+1

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def minDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        if None in [root.left, root.right]:
            return max(self.minDepth(root.left), self.minDepth(root.right)) + 1
        else:
            return min(self.minDepth(root.left), self.minDepth(root.right)) + 1
```