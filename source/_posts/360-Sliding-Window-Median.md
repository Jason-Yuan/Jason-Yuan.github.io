title: "360 Sliding Window Median"
date: 2015-10-11 15:18:31
tags: ["LintCode", "Array", "Median", "Hash Heap", "Heap"]
categories: "LintCode"
---

# 原题
>给定一个包含 n 个整数的数组，和一个大小为 k 的滑动窗口,从左到右在数组中滑动这个窗口，找到数组中每个窗口内的最大值。(如果数组中有偶数，则在该窗口存储该数字后，返回第 N/2-th 个数字。)

对于数组 [1,2,7,8,5], 滑动大小 k = 3 的窗口时，返回 [2,7,7]
最初，窗口的数组是这样的：
[ ` 1,2,7 ` ,8,5] , 返回中位数 2;
接着，窗口继续向前滑动一次。
[1, ` 2,7,8 ` ,5], 返回中位数 7;
接着，窗口继续向前滑动一次。
[1,2, ` 7,8,5 ` ], 返回中位数 7;

# 解题思路
* 类似题是[Data Stream Median]，[Data Stream Median]维护了一个max heap和一个min heap，不需要删除操作
* 本题是滑动窗口类型题目，分为两个操作，加入新元素，删除头部元素
* 由于涉及删除元素，所以要维护一个`hash max heap`和一个`hash min heap`

# 完整代码
```python
class node:
    """
    create a node class to handle the duplicates in heap.
    id => the index of x, num = number of x in heap
    """
    def __init__(self, id, number):
        self.id = id
        self.num = number

class HashHeap:
    def __init__(self):
        self.map = {}
        self.hashmaxheap = [0]
        self.map[0] = node(0, 1)
        self.currentSize = 0

    def put(self, data):
        """add a new item to the hashmaxheap"""
        if data in self.map:
            existData = self.map[data]
            self.map[data] = node(existData.id, existData.num + 1)
            self.currentSize += 1
            return 
        else:
            self.hashmaxheap.append(data)
            self.map[data] = node(len(self.hashmaxheap) - 1, 1)
            self.currentSize += 1
            self.siftUp(len(self.hashmaxheap) - 1)

    def peek(self):
        """returns the item with the maxmum key value"""
        return self.hashmaxheap[1]

    def get(self):
        """returns the item with the maxmum key value, removing the item from the heap"""
        res = self.hashmaxheap[1]
        if self.map[res].num == 1:
            if self.map[res].id == len(self.hashmaxheap) - 1:
                del self.map[res]
                self.hashmaxheap.pop()
                self.currentSize -= 1
                return res
            del self.map[res]
            self.hashmaxheap[1] = self.hashmaxheap[-1]
            self.map[self.hashmaxheap[1]] = node(1, self.map[self.hashmaxheap[1]].num)
            self.hashmaxheap.pop()
            self.siftDown(1)
        else:
            self.map[res] = node(1, self.map[res].num - 1)
        self.currentSize -= 1
        return res

    def delete(self, data):
        existData = self.map[data]
        if existData.num == 1:
            del self.map[data]
            if existData.id == len(self.hashmaxheap) - 1:
                self.hashmaxheap.pop()
                self.currentSize -= 1
                return
            self.hashmaxheap[existData.id] = self.hashmaxheap[-1]
            self.map[self.hashmaxheap[-1]] = node(existData.id, self.map[self.hashmaxheap[-1]].num)
            self.hashmaxheap.pop()
            self.siftUp(existData.id)
            self.siftDown(existData.id)
        else:
            self.map[data] = node(existData.id, existData.num - 1)
        self.currentSize -= 1

    def siftUp(self, index):
        # // means devide by 2 and return int
        while index // 2 > 0:
            if self.hashmaxheap[index] < self.hashmaxheap[index // 2]:
                break
            else:
                numA = self.map[self.hashmaxheap[index]].num
                numB = self.map[self.hashmaxheap[index // 2]].num
                self.map[self.hashmaxheap[index]] = node(index // 2, numA)
                self.map[self.hashmaxheap[index // 2]] = node(index, numB)
                self.hashmaxheap[index], self.hashmaxheap[index // 2] = self.hashmaxheap[index // 2], self.hashmaxheap[index] 
            index = index // 2

    def siftDown(self, index):
        """correct single violation in a sub-tree"""
        if index > (len(self.hashmaxheap) - 1) // 2:
            return
        # find the max child of current index
        if (index * 2 + 1) > (len(self.hashmaxheap) - 1) or self.hashmaxheap[index * 2] > self.hashmaxheap[index * 2 + 1]:
            maxChild = index * 2
        else:
            maxChild = index * 2 + 1
        if self.hashmaxheap[index] > self.hashmaxheap[maxChild]:
            return
        else:
            numA = self.map[self.hashmaxheap[index]].num
            numB = self.map[self.hashmaxheap[maxChild]].num
            self.map[self.hashmaxheap[index]] = node(maxChild, numA)
            self.map[self.hashmaxheap[maxChild]] = node(index, numB)
            self.hashmaxheap[index], self.hashmaxheap[maxChild] = self.hashmaxheap[maxChild], self.hashmaxheap[index] 
        self.siftDown(index * 2)
        self.siftDown(index * 2 + 1)

    def size(self):
        return self.currentSize

    def isEmpty(self):
        return self.currentSize == 0
        
class Solution:
    """
    @param nums: A list of integers.
    @return: The median of element inside the window at each moving.
    """
    def medianSlidingWindow(self, nums, k):
        res = []
        if not nums:
            return res
            
        median = nums[0]
        minHeap = HashHeap()
        maxHeap = HashHeap()
        for i in range(len(nums)):
            if i != 0:
                if nums[i] > median:
                    minHeap.put(- nums[i])
                else:
                    maxHeap.put(nums[i])
            if i >= k:
                if median == nums[i - k]:
                    if not maxHeap.isEmpty():
                        median = maxHeap.get()
                    elif not minHeap.isEmpty():
                        median = - minHeap.get()
                elif median < nums[i - k]:
                    minHeap.delete(- nums[i - k])
                else:
                    maxHeap.delete(nums[i - k])
                    
            while maxHeap.size() > minHeap.size():
                minHeap.put(- median)
                median = maxHeap.get()
            
            while minHeap.size() > maxHeap.size() + 1:
                maxHeap.put(median)
                median = - minHeap.get()
                
            if i + 1 >= k:
                res.append(median)
                
        return res
```