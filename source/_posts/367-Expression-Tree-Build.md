title: "367 Expression Tree Build"
date: 2015-10-09 02:45:50
tags: ["LintCode", "Expression Tree", "Stack", "Min Tree", "Recursion"]
categories: "LintCode"
---

# 原题
>表达树是一个二叉树的结构，用于衡量特定的表达。所有表达树的叶子都有一个数字字符串值。而所有表达树的非叶子都有另一个操作字符串值。
给定一个表达数组，请构造该表达的表达树，并返回该表达树的根。

对于 `(2*6-(23+7)/(1+2))` 的表达（可表示为 ["2" "*" "6" "-" "(" "23" "+" "7" ")" "/" "(" "1" "+" "2" ")"]). 其表达树如下：
```
                 [ - ]
              /          \
         [ * ]              [ / ]
       /     \           /         \
     [ 2 ]  [ 6 ]      [ + ]        [ + ]
                      /    \       /      \
                    [ 23 ][ 7 ] [ 1 ]   [ 2 ] 
```
在构造该表达树后，你只需返回根节点[-]。

# 解题思路
* 首先定义一个`MyTreeNode`，对于每一个符号或者数字，附上一个val，以便于之后evaluate它，来建立树
* 如果`expression[i]`等于"("或者")"调整base然后`continue`跳过后面的步骤
* 维护一个递增stack，建立**最小树**，类似题[Max Tree]
* 最后递归copy已经建立的树，给`self.exp_node`赋值

# 完整代码
```python
"""
Definition of ExpressionTreeNode:
class ExpressionTreeNode:
    def __init__(self, symbol):
        self.symbol = symbol
        self.left, self.right = None, None
"""
class MyTreeNode:
    def __init__(self, val, s):
        self.left = None
        self.right = None
        self.val = val
        self.exp_node = ExpressionTreeNode(s)

class Solution:
    # @param expression: A string list
    # @return: The root of expression tree
    def get_val(self, a, base):
        if a == '+' or a == '-':
            if base == sys.maxint:
                return base
            return 1 + base
        if a == '*' or a == '/':
            if base == sys.maxint:
                return base
            return 2 + base
        return sys.maxint
    
    def create_tree(self, expression):
        stack = []
        base = 0
        for i in range(len(expression)):
            if expression[i] == '(':
                if base != sys.maxint:
                    base += 10
                continue
            elif expression[i] == ')':
                if base != sys.maxint:
                    base -= 10
                continue
            val = self.get_val(expression[i], base)
    
            node = MyTreeNode(val, expression[i])
            while stack and val <= stack[-1].val:
                node.left = stack.pop()
            if stack:
                stack[-1].right = node
            stack.append(node)
        if not stack:
            return None
        return stack[0]
    
    def copy_tree(self, root):
        if not root:
            return None
        root.exp_node.left = self.copy_tree(root.left)
        root.exp_node.right = self.copy_tree(root.right)
        return root.exp_node
    
    def build(self, expression):
        # write your code here
        root = self.create_tree(expression)
        return self.copy_tree(root)
```