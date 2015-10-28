title: "126 Max Tree"
date: 2015-09-27 23:29:33
tags: ["LintCode", "Stack", "Descending order stack"]
categories: "LintCode"
---

# 原题
>给出一个没有重复的整数数组，在此数组上建立最大树的定义如下：
1.根是数组中最大的数
2.左子树和右子树元素分别是被父节点元素切分开的子数组中的最大值
利用给定的数组构造最大树。

给出数组 [2, 5, 6, 0, 3, 1]，构造的最大树如下：
```
         6
        / \
       5   3
      /   / \
     2   0   1
```

# 解题思路
* 通过观察发现规律，对于每个node的父亲节点 = min(左边第一个比它大的，右边第一个比它大的)
* 维护一个降序数组，可以实现对这个min的快速查找
```
# stack = [2], push 5 因为 5 > 2, 所以2是5的左儿子, pop 2
# stack = [], push 5
# stack = [5], push 6, 因为 6 > 5，所以5是6的左儿子, pop 5
# stack = [], push 6
# stack = [6], push 0, 因为0 < 6, stack = [6], 所以0是6的右儿子
# stack = [6, 0], push 3, 因为3 > 0, 所以0是3的左儿子， pop 0
# stack = [6], 所以3是6的右儿子， push 3
# stack = [6, 3], push 1, 因为1 < 3，所以1是3的右儿子
```

# 完整代码
```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
class Solution:
    # @param A: Given an integer array with no duplicates.
    # @return: The root of max tree.
    def maxTree(self, A):
        # write your code here
        stack = []
        for element in A:
            node = TreeNode(element)
            while len(stack) != 0 and element > stack[-1].val:
                node.left = stack.pop()
            if len(stack) != 0:
                stack[-1].right = node
            stack.append(node)
        return stack[0]
```