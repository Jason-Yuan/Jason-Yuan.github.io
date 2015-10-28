title: "155 Min Stack"
date: 2015-09-27 23:28:09
tags: ["LeetCode", "Stack"]
categories: "LeetCode"
---

# 原题
>实现一个带有取最小值min方法的栈，min方法将返回当前栈中的最小值。你实现的栈将支持**push**，**pop**和**min**操作，所有操作要求都在O(1)时间内完成。

# 解题思路
* 使用两个stack实现， 一个stack正常存储数值，另一个stack存储每次的最小值（保存最小值的变化序列）

# 完整代码
```python
class MinStack(object):
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.minstack = []
        self.stack = []

    def push(self, x):
        """
        :type x: int
        :rtype: nothing
        """
        self.stack.append(x)
        if len(self.minstack) == 0 or self.minstack[-1] > x:
            self.minstack.append(x)
        else:
            self.minstack.append(self.minstack[-1])

    def pop(self):
        """
        :rtype: nothing
        """
        self.minstack.pop()
        return self.stack.pop()

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1]

    def getMin(self):
        """
        :rtype: int
        """
        return self.minstack[-1]
```