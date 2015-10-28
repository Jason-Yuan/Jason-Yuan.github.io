title: "98 Validate Binary Search Tree"
date: 2015-10-21 02:52:44
tags: ["LeetCode", "BST", "Divide and Conquer", "Traversal", "DFS", "Recursion"]
categories: "LeetCode"
---

# 原题
>给定一个二叉树，判断它是否是合法的二叉查找树(BST)
一棵BST定义为：
节点的左子树中的值要**严格小于**该节点的值。
节点的右子树中的值要**严格大于**该节点的值。
左右子树也必须是二叉查找树。

一个例子：
```
   1
  / \
 2   3
    /
   4
    \
     5
```
上述这棵二叉树序列化为"{1,2,3,#,#,4,#,#,5}".

# 解题思路
* BST的中序遍历结果是一个**升序序列**
所以可以转化为数组，再遍历一遍数组，看是否升序。时间空间复杂度都是O(n)
* divide & conquer
 * result每次返回一个区间的最大，最小
 * conquer的时候判断左子树
* 递归判断`min < left.val < node.val < right.val < max`

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
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        res = []
        self.inOrderToArray(root, res)
        for i in range(1, len(res)):
            if res[i-1] >= res[i]:
                return False
        return True
        
    def inOrderToArray(self, node, array):
        if node:
            if node.left:
                self.inOrderToArray(node.left, array)
            array.append(node.val)
            if node.right:
                self.inOrderToArray(node.right, array)

# 方法二
class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        res, _, _ = self.Helper(root)
        return res
    
    def Helper(self, root):
        if not root:
            return True, - sys.maxint, sys.maxint
        
        left = self.Helper(root.left)
        right = self.Helper(root.right)
        
        if not left[0] or not right[0]:
            return False, 0, 0
        
        if (root.left != None and left[1] >= root.val) or (root.right != None and right[2] <= root.val):
            return False, 0, 0
        
        return True, max(root.val, right[1]), min(root.val, left[2])

# 方法三
class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        return self.helper(root)  

    def helper(self, root, min=-float("inf"), max=float("inf")):
	    if root is None:
		    return True
	    elif root.left == None and root.right == None:
		    return root.val > min and root.val < max
	    elif root.left == None:
		    return self.helper(root.right, root.val, max) and root.val > min
	    elif root.right == None:
		    return self.helper(root.left, min, root.val) and root.val < max
	    return self.helper(root.left, min, root.val) and self.helper(root.right, root.val, max)
```