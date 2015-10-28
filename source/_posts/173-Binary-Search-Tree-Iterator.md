title: "173 Binary Search Tree Iterator"
date: 2015-10-21 03:38:56
tags: ["LeetCode", "DFS", "Non-recursion", "BST"]
categories: "LeetCode"
---

# 原题
>设计实现一个带有下列属性的二叉查找树的迭代器：
元素按照递增的顺序被访问（比如中序遍历）
next()和hasNext()的询问操作要求**均摊**时间复杂度是O(1)

对于下列二叉查找树，使用迭代器进行中序遍历的结果为 [3, 6, 7, 8, 9, 10, 11, 12]
```
     10
   /    \
  6      11
 / \       \
3   9       12
   /
  8
 /
7
```

# 解题思路
* 本题相当于考察了BST的非递归中序遍历
* 需要maintain一个stack，首先从root开始push入栈直到最左节点
初始stack为：
```
10, 6, 3
```
* 在遍历过程中，如果某个节点存在右儿子，则继续从右儿子开始push入栈直到其最左节点
result = 3, 6
因为6有右儿子，所以6被pop出去之后，从6为root开始push入栈直到最左节点，然后stack为：
```
10， 9， 8， 7
```

# 完整代码
```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None

Example of iterate a tree:
iterator = Solution(root)
while iterator.hasNext():
    node = iterator.next()
    do something for node 
"""
class Solution:
    def __init__(self, root):
        self.stack = []
        while root:
            self.stack.append(root)
            root = root.left

    # @return a boolean, whether we have a next smallest number
    def hasNext(self):
        return True if self.stack else False

    # @return an node
    def next(self):
        current = self.stack.pop()
        node = current.right
        while node:
            self.stack.append(node)
            node = node.left
        return current
```