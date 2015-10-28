title: "130 Heapify"
date: 2015-09-27 23:44:49
tags: ["LintCode", "Priority Queue", "Heapify"]
categories: "LintCode"
---

# 原题
>给出一个整数数组，堆化操作就是把它变成一个最小堆数组。
对于堆数组A，A[0]是堆的根，并对于每个A[i]，A [i * 2 + 1]是A[i]的左儿子并且A[i * 2 + 2]是A[i]的右儿子。

给出** [3,2,1,4,5]**，返回**[1,2,3,4,5] **或者任何一个合法的堆数组

# 解题思路
* minHeapify就相当于sift down函数，minHeapify函数可以写成recursion也可以写成while循环
* heapify就相当于buildMinHeap函数
* 注意如果想要在函数中整体覆盖一个数组，Python的写法如下
```
# 正确
array[:] = newArray[:]
# 两个错误示范，相当于在函数内改变了reference指向，并没有实际改变原来array指向的object
array = newArray[:]
array = newArray
```

# 完整代码
```python
class Solution:
    # @param A: Given an integer array
    # @return: void
    def heapify(self, A):
        self.currentSize = len(A)
        self.minheap = [0] + A
        for i in range(len(A)//2, 0, -1):
            self.minHeapify(self.minheap, i)
        A[:] = self.minheap[1:]
        
    def minHeapify(self, L, index):
        if index > self.currentSize // 2:
            return
        if index*2+1 > self.currentSize or L[index*2] < L[index*2+1]:
            minChild = index*2
            if L[index] > L[minChild]:
                L[index], L[minChild] = L[minChild], L[index]
            self.minHeapify(L, index*2)
        else:
            minChild = index*2+1
            if L[index] > L[minChild]:
                L[index], L[minChild] = L[minChild], L[index]
            self.minHeapify(L, index*2+1)
```