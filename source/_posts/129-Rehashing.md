title: "129 Rehashing"
date: 2015-09-27 23:39:19
tags: ["LintCode", "Hash", "Rehashing"]
categories: "LintCode"
---

# 原题
>哈希表容量的大小在一开始是不确定的。如果哈希表存储的元素太多（如超过容量的十分之一），我们应该将哈希表容量扩大一倍，并将所有的哈希值重新安排。

假设你有如下一哈希表：size=3, capacity=4
给出 [null, 21->9->null, 14->null, null]
返回 [null, 9->null, null, null, null, 21->null, 14->null, null]

# 解题思路
* 第一步容量扩大一倍，loop之前hash表的所有listnode，逐个拷贝

# 完整代码
```python
"""
Definition of ListNode
class ListNode(object):

    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""
class Solution:
    """
    @param hashTable: A list of The first node of linked list
    @return: A list of The first node of linked list which have twice size
    """
    def rehashing(self, hashTable):
        if len(hashTable) <= 0:
            return hashTable
            
        newcapacity = 2 * len(hashTable)
        newTable = [None for i in range(newcapacity)]
        for listNode in hashTable:
            while listNode != None:
                newindex = (listNode.val % newcapacity + newcapacity) % newcapacity
                if newTable[newindex] == None:
                    newTable[newindex] = ListNode(listNode.val)
                else:
                    dummy = newTable[newindex]
                    while dummy.next != None:
                        dummy = dummy.next
                    dummy.next = ListNode(listNode.val)
                listNode = listNode.next
        return newTable
```