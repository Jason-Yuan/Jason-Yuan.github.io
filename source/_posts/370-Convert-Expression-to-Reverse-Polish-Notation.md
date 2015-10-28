title: "370 Convert Expression to Reverse Polish Notation"
date: 2015-10-10 17:35:25
tags: ["LintCode", "Array", "Stack", "DFS"]
categories: "LintCode"
---

# 原题
>给定一个表达式字符串数组，返回该表达式的逆波兰表达式（即去掉括号）。

对于 [3 - 4 + 5]的表达式（该表达式可表示为["3", "-", "4", "+", "5"]），返回 [3 4 - 5 +]（该表达式可表示为 ["3", "4", "-", "5", "+"]）。

# 解题思路
* 首先建立表达式树，如题[Expression Tree Build]
* Reverse Polish Notation即表达式树后序遍历的结果

# 完整代码
```python
class expressionTreeNode:
    def __init__(self, symbol):
        self.symbol = symbol
        self.left, self.right = None, None

class MyNode:
    def __init__(self, val, s):
        self.left = None
        self.right = None
        self.val = val
        self.exp_node = expressionTreeNode(s)

class Solution:
    # @param expression: A string list
    # @return: The Reverse Polish notation of this expression
    def get_val(self, a, base):
        if a == '+' or a == '-':
            return 1 + base
        if a == '*' or a == '/':
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
    
            node = MyNode(val, expression[i])
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
        root = self.create_tree(expression)
        return self.copy_tree(root)
        
    def dfs(self, root, list):
		if root == None:
			return
		if root.left:
			self.dfs(root.left, list)
		if root.right:
			self.dfs(root.right, list)
		list.append(root.symbol)
        
    def convertToRPN(self, expression):
        res = []
        root = self.build(expression)
        self.dfs(root, res)
        return res
```