title: "107 Binary Tree Level Order Traversal II"
date: 2015-10-21 01:38:34
tags: ["LeetCode", "Binary Tree", "BFS"]
categories: "LeetCode"
---

# 原题
>给出一棵二叉树，返回其节点值从底向上的层次序遍历（按从叶节点所在层到根节点所在的层遍历，然后逐层从左往右遍历）

给出一棵二叉树 {3,9,20,#,#,15,7}
```
    3
   / \
  9  20
    /  \
   15   7
```
按照从下往上的层次遍历为：
```
[
  [15,7],
  [9,20],
  [3]
]
```

# 解题思路
* 本题与[Binary Tree Level Order Traversal I]一样，同样使用一个queue解决，只是在生成result的时候不是`append`而是`insert(0, temp)`
* 每次从见面加入`temp`结果

# 完整代码
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import Queue

class Solution(object):
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
            
        result = []
        MyQueue = Queue.Queue()
        MyQueue.put(root)
        while not MyQueue.empty():
            temp = []
            size = MyQueue.qsize()
            for i in range(size):
                node = MyQueue.get()
                if node.left:
                    MyQueue.put(node.left)
                if node.right:
                    MyQueue.put(node.right)
                temp.append(node.val)
            result.insert(0, temp)
            
        return result
```