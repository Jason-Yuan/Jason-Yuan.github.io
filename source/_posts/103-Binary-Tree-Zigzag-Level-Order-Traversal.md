title: "103 Binary Tree Zigzag Level Order Traversal"
date: 2015-10-21 02:02:13
tags: ["LeetCode", "Binary Tree", "BFS"]
categories: "LeetCode"
---

# 原题
>给出一棵二叉树，返回其节点值的锯齿形层次遍历（先从左往右，下一层再从右往左，层与层之间交替进行） 

给出一棵二叉树 {3,9,20,#,#,15,7}
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其锯齿形的层次遍历为：
```
[
  [3],
  [20,9],
  [15,7]
]
```

# 解题思路
* 相似题[Binary Tree Level Order Traversal]和[Binary Tree Level Order Traversal II]
* 同样是一个queue实现BFS，只是在处理temp结果的时候每次改变顺序，借助一个`flag`变量

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
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
            
        result = []
        MyQueue = Queue.Queue()
        MyQueue.put(root)
        flag = True
        while not MyQueue.empty():
            size = MyQueue.qsize()
            temp = []
            for i in range(size):
                node = MyQueue.get()
                if node.left:
                    MyQueue.put(node.left)
                if node.right:
                    MyQueue.put(node.right)
                if flag:
                    temp.append(node.val)
                else:
                    temp.insert(0, node.val)
            result.append(temp)
            flag = not flag

        return result
```