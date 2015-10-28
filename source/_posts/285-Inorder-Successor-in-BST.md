title: "285 Inorder Successor in BST"
date: 2015-10-04 18:37:58
tags: ["LeetCode", "BST"]
categories: "LeetCode"
---

# 原题
>Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Given tree = [2,1] and node = 1:
```
   2
  /
 1
```
return node 2.
Given tree = [2,1,3] and node = 2:
```
    2
   / \
  1   3
```
return node 3.

# 解题思路
* successor变量记录**root**的父亲结点，当while循环结束时，如果原本的root = None或者p不存在与BST中(那么此刻root = None)，都返回None
* 找到p之后，如果p没有右儿子，则第一个比它大的数字就是刚刚记录的successor
* 找到p之后，如果有右儿子，则找到右子树中的**最左边的值**(最小值)

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    """
    @param root <TreeNode>: The root of the BST.
    @param p <TreeNode>: You need find the successor node of p.
    @return <TreeNode>: Successor of p.
    """
    def inorderSuccessor(self, root, p):
        successor = None
        while root != None and root.val != p.val:
            if root.val > p.val:
                successor = root
                root = root.left
            else:
                root = root.right
        
        # 原本的root = None 或者 p不存在与BST中，此刻root = None
        if root == None:
            return None
                
        # 找到p之后，如果p没有右儿子，则第一个比它大的数字就是刚刚记录的successor
        if root.right == None:
            return successor
        
        # 找到p之后，如果有右儿子，则找到右子树中的最左边的值（最小值）
        root = root.right
        while root.left != None:
            root = root.left
        
        return root
```