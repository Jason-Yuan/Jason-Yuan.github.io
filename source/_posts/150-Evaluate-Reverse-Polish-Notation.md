title: "150 Evaluate Reverse Polish Notation"
date: 2015-10-10 19:18:52
tags: ["LeetCode", "Array", "Stack"]
categories: "LeetCode"
---

# 原题
>对于一个逆波兰数学表达式求值

["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6

# 解题思路
* 使用stack，遍历逆波兰表达式
* 遇到`+-*/`则从stack中pop出两个数做相应的运算，结果push入栈
* 遇到数字直接入栈

# 完整代码
```python
class Solution(object):
    def evalRPN(self, tokens):
        """
        :type tokens: List[str]
        :rtype: int
        """
        stack = []
        operators = "+-*/"
        for str in tokens:
            if str not in operators:
                stack.append(int(str))
            else:
                a = stack.pop()
                b = stack.pop()
                if str == "+":
                    stack.append(a + b)
                elif str == "-":
                    stack.append(b - a)
                elif str == "*":
                    stack.append(a * b)
                elif str == "/":
                    if a * b < 0 and b % a != 0:
                        stack.append(b / a + 1)
                    else:
                        stack.append(b / a)
        if not stack:
            return 0
        return stack.pop()
```