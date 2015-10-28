title: "81 Data Stream Median"
date: 2015-09-27 23:45:51
tags: ["LintCode", "Priority Queue", "MaxHeap", "MinHeap", "Queue"]
categories: "LintCode"
---

# 原题
>数字是不断进入数组的，在每次添加一个新的数进入数组的同时返回当前新数组的中位数。

持续进入数组的数的列表为：**[1, 2, 3, 4, 5]**，则返回[**1, 1, 2, 2, 3]**
持续进入数组的数的列表为：**[4, 5, 1, 3, 2, 6, 0]**，则返回 **[4, 4, 4, 3, 3, 3, 3]**
持续进入数组的数的列表为：**[2, 20, 100]**，则返回**[2, 2, 20]**

时间复杂度为O(nlogn)

# 解题思路
* Priority Queue
* 维护一个MaxHeap和一个MinHeap
* size(MinHeap) - 1 < size(MaxHeap) < size(MaxHeap)
* MaxHeap可以通过MinHeap实现，每次put相反数入队
* python中Priority Queue的使用
```python
import Queue
q = Queue.PriorityQueue()
q.put(3)
q.put(1)
q.put(2)
a = q.get()
print a # output = 1
```

# 完整代码
```python
import Queue

class Solution:
    """
    @param nums: A list of integers.
    @return: The median of numbers
    """
    def medianII(self, nums):
        result = []
        if not nums:
            return result
            
        median = nums[0]
        MaxHeap, MinHeap = Queue.PriorityQueue(), Queue.PriorityQueue()
        result.append(median)
        
        for i in range(1, len(nums)):
            if nums[i] > median:
                MinHeap.put(nums[i])
            else:
                MaxHeap.put(-nums[i])
                
            if MaxHeap.qsize() > MinHeap.qsize():
                MinHeap.put(median)
                median = -MaxHeap.get()
            if MaxHeap.qsize() + 1 < MinHeap.qsize():
                MaxHeap.put(-median)
                median = MinHeap.get()
            result.append(median)
            
        return result
```