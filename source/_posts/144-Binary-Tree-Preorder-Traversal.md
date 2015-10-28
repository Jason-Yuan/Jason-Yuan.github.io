title: "144 Binary Tree Preorder Traversal"
date: 2015-10-20 23:24:32
tags: ["LeetCode", "BST", "Preorder", "Recursion", "Divide and Conquer", "Non-recursion"]
categories: "LeetCode"
---

# 原题
>给出一棵二叉树，返回其节点值的前序遍历。

给出一棵二叉树 {1,#,2,3}
```
   1
    \
     2
    /
   3
```
返回 [1,2,3]

# 解题思路
* helper函数中`if root:`即表明root如果不是None，就进行父/左/右的添加
* 方法一，最简单，recursion (no return value)
* 方法二，divide & conquer (has return value)
* 方法三，非递归，因为递归调用了系统中的栈空间，所以非递归的实现要用到stack这种数据结构

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

# 方法一
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        ret = []
        self.RecPreOrderTraversal(root, ret)
        return ret
        
    def RecPreOrderTraversal(self, root, result):
        if root != None: # skip None or Leaf
            result.append(root.val)
            self.RecPreOrderTraversal(root.left, result)
            self.RecPreOrderTraversal(root.right, result)

# 方法二
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        if root is None:
            return res
        
        left = self.preorderTraversal(root.left)
        right = self.preorderTraversal(root.right)
        
        res.append(root.val)
        res += left
        res += right
        return res 

# 方法三
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []
        stack = [root]
        preorder = []
        while stack:
            node = stack.pop()
            preorder.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return preorder
```