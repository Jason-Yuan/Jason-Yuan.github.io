title: "6 Merge Sorted Array II"
date: 2015-10-12 16:01:32
tags: ["LintCode", "Array", "Merge"]
categories: "LintCode"
---

# 原题
>合并两个排序的整数数组A和B变成一个新的数组。

给出A=[1,2,3,4]，B=[2,4,5,6]，返回 [1,2,2,3,4,4,5,6]

# 解题思路
* 不同于[Merge Sorted Array I], 由于merge到一个新的数组，所以从前往后，小的出列
```python
headA = 0
headB = 0
```
* 方法二：合并，排序

# 完整代码
```python
# 方法一
class Solution:
    #@param A and B: sorted integer array A and B.
    #@return: A new sorted integer array
    def mergeSortedArray(self, A, B):
        headA = 0
        headB = 0
        res = []
        while headA <= len(A) - 1 and headB <= len(B) - 1:
            if A[headA] <= B[headB]:
                res.append(A[headA])
                headA += 1
            else:
                res.append(B[headB])
                headB += 1
        
        while headA <= len(A) - 1:
            res.append(A[headA])
            headA += 1
            
        while headB <= len(B) - 1:
            res.append(B[headB])
            headB += 1
            
        return res

# 方法二
class Solution:
    #@param A and B: sorted integer array A and B.
    #@return: A new sorted integer array
    def mergeSortedArray(self, A, B):
        C = A + B
        return sorted(C)
```