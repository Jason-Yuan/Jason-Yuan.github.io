title: "28 Implement strStr()"
date: 2015-10-11 15:56:15
tags: ["LeetCode", "Two Pointer", "String"]
categories: "LeetCode"
---

# 原题
>字符串查找（又称查找子字符串），是字符串操作中一个很有用的函数。你的任务是实现这个函数。
对于一个给定的 source 字符串和一个 target 字符串，你应该在 source 字符串中找出 target 字符串出现的第一个位置(从0开始)。
如果不存在，则返回 -1。

如果 source = "source" 和 target = "target"，返回 -1。
如果 source = "abcdabcdefg" 和 target = "bcd"，返回 1。

# 解题思路
* 双层for循环, O(m*n)
* 注意一些边界情况，主要是开头的处理
* `haystack == None , needle == None` 要返回`-1`
* `haystack == "" , needle == ""` 要返回`0`而不是`-1`
* `needle = ""` 要返回`0`

# 完整代码
```python
class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        if haystack == None or needle == None or len(haystack) < len(needle):
            return -1
        if haystack == needle or needle == "":
            return 0
        for i in range(len(haystack) - len(needle) + 1):
            flag = True
            res = i
            for char in needle:
                if char == haystack[i]:
                    i += 1
                else:
                    flag = False
            if flag:
                return res
        return -1
```