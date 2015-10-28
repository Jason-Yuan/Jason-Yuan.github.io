title: "447 Search in a Big Sorted Array"
date: 2015-10-03 21:59:06
tags: ["LintCode", "Array", "Binary Search"]
categories: "LintCode"
---

# 原题
>Given a big sorted array, find the first index of a target number. Your algorithm should be in O(log k), where k is the first index of the target number.
Return -1, if the number doesn't exist in the array.

# 解题思路
* 二分查找
* 由于有重复数字时，返回第一个出现的index，所以`A[mid] == target`时，`end = mid`
* 由于数组非常大，所以我们希望用一种方式找到刚刚好比target大一些的数作为end，代码如下：
```python
end = 0
while end < len(A) and A[end] < target:
    end = end * 2 + 1
if end >= len(A):
    end = len(A) - 1
```

# 完整代码
```python
class Solution:
    # @param {int[]} A an integer array
    # @param {int} target an integer
    # @return {int} an integer
    def searchBigSortedArray(self, A, target):
        if not A:
            return -1
            
        end = 0
        while end < len(A) and A[end] < target:
            end = end * 2 + 1
        if end >= len(A):
            end = len(A) - 1
        
        start = 0
        while start + 1 < end:
            mid = start + (end - start) / 2
            if A[mid] >= target:
                end = mid
            else:
                start = mid
                
        if A[start] == target:
            return start
        elif A[end] == target:
            return end
                
        return -1
```