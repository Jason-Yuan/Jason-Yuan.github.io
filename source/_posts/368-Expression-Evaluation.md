title: "368 Expression Evaluation"
date: 2015-10-10 18:05:58
tags: ["LintCode", "Array", "Stack", "DFS"]
categories: "LintCode"
---

# 原题
>给一个用字符串表示的表达式数组，求出这个表达式的值。

对于表达式 (2*6-(23+7)/(1+2)), 对应的数组为：
```
[
  "2", "*", "6", "-", "(",
  "23", "+", "7", ")", "/",
  (", "1", "+", "2", ")"
]
```
其值为 2

# 解题思路
* str与int相互转化
```
num = int("15") // num = 15
s = str(num) // s = "15"
```
* 表达式 -> 建立表达式树
* 表达式树 -> 求逆波兰表达式
* 逆波兰表达式 -> 求值(使用stack)。遍历**逆波兰表达式**，遇到`+-*/`除pop两个数做相应的运算，结果入栈。如果遇到数字，直接push入栈

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
    # @param expression: a list of strings;
    # @return: an integer
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
        
    def evaluateExpression(self, expression):
        reversePolishNotation = self.convertToRPN(expression)
        stack = []
        operators = "+-*/"
        for str in reversePolishNotation:
            if str not in operators:
                stack.append(int(str))
            else:
                a = stack.pop()
                b = stack.pop()
                if str == "+":
                    stack.append(a + b)
                elif str == "-":
                    stack.append(b - a)
                elif str == "/":
                    stack.append(b / a)
                elif str == "*":
                    stack.append(a * b)
        if not stack:
            return 0
        return stack[-1]
```