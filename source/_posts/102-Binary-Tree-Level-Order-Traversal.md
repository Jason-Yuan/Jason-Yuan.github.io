title: "102 Binary Tree Level Order Traversal"
date: 2015-10-21 01:29:57
tags: ["LeetCode", "Binary Tree", "BFS"]
categories: "LeetCode"
---

# 原题
>给出一棵二叉树，返回其节点值的层次遍历（逐层从左往右访问）

给一棵二叉树 {3,9,20,#,#,15,7} ：
```
  3
 / \
9  20
  /  \
 15   7
```
返回他的分层遍历结果：
```
[
  [3],
  [9,20],
  [15,7]
]
```

# 解题思路
* 实现方式有三种
 * 两个queue
 * 一个queue + dummy node
 * 一个queue
* 对于使用一个queue的实现方式为，每次先求size，即这一层多少node。然后再把这一层node的value加入temp组成数组加入到最后的结果中，把这一层node的左右儿子加入到queue中

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
    def levelOrder(self, root):
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
            size = MyQueue.qsize()
            temp = []
            for i in range(size):
                node = MyQueue.get()
                if node.left:
                    MyQueue.put(node.left)
                if node.right:
                    MyQueue.put(node.right)
                temp.append(node.val)
            result.append(temp)
            
        return result
```