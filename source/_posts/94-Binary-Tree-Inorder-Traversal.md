title: "94 Binary Tree Inorder Traversal"
date: 2015-10-22 02:58:13
tags: ["LeetCode", "BST", "Divide and Conquer", "Recursion", "Non-recursion"]
categories: "LeetCode"
---


# 原题
>给出一棵二叉树,返回其中序遍历

给出二叉树 {1,#,2,3}
```
   1
    \
     2
    /
   3
```
返回 [1,3,2]

# 解题思路
* 方法一：递归，定义helper函数
* 方法二：Divide & Conquer
* 方法二：非递归
 * (操作一)每一次从当前节点开始(第一次是root节点)遍历至最左节点，一次入栈。
 * (操作二)pop出栈一个node，node.val加入到结果，然后看该node有没有右儿子
  * 有的话重复操作一
  * 没有的话，继续pop出栈一个node，重复操作二
* 非递归解法可以体会一下另外一个题[Inorder Successor in Binary Search Tree]

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
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        self.helper(root, res)
        return res
        
    def helper(self, root, result):
        if root != None:
            self.helper(root.left, result)
            result.append(root.val)
            self.helper(root.right, result)

# 方法二
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        result = []
        if root == None:
            return result
        result.append(root.val)
        
        # divide
        left = self.inorderTraversal(root.left)
        right = self.inorderTraversal(root.right)
        
        # conquer
        return left + result + right

# 方法三
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        stack = []
        result = []
        current = root
        while current != None or len(stack) != 0:
            while current != None:
                stack.append(current)
                current = current.left
            current = stack[-1];
            stack.pop();
            result.append(current.val);
            current = current.right;
        return result
```