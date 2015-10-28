title: "146 LRU Cache"
date: 2015-09-27 23:43:13
tags: ["LeetCode", "LRU", "Cache", "Hash", "Linked List"]
categories: "LeetCode"
---

# 原题
>为最近最少使用（LRU）缓存策略设计一个数据结构，它应该支持以下操作：获取数据（**get**）和写入数据（**set**）。
1.获取数据**get(key)
**：如果缓存中存在key，则获取其数据值（通常是正数），否则返回-1。
2.写入数据**set(key, value)**：如果key还没有在缓存中，则写入其数据值。当缓存达到上限，它应该在写入新数据之前删除最近最少使用的数据用来腾出空闲位置。

# 解题思路
* 第一过程中涉及membership的查询 -> Hash Table
* 第二过程中涉及一个个的**增**、**删** -> Linked List (数组不适合，数组适合一批一批的增删)
* Linked List中的每个node要有next和prev属性，要同时记录Dummy(head)和tail

* 本解法采用的singly linked list所以**hash表里面存的key指向的是前一个node**

# 完整代码
```python
class LinkedNode:

    def __init__(self, key=None, value=None, next=None):
        self.key = key
        self.value = value
        self.next = next
        
class LRUCache(object):

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.hash = {}
        self.head = LinkedNode()
        self.tail = self.head
        self.capacity = capacity
        
    def push_back(self, node):
        self.hash[node.key] = self.tail
        self.tail.next = node
        self.tail = node
        
    def pop_front(self):
        del self.hash[self.head.next.key]
        self.head.next = self.head.next.next
        self.hash[self.head.next.key] = self.head
        
    # change "prev->node->next...->tail"
    # to "prev->next->...->tail->node"
    def kick(self, prev):
        node = prev.next
        if node == self.tail:
            return
        prev.next = node.next
        if node.next is not None:
            self.hash[node.next.key] = prev
            node.next = None
        self.push_back(node)

    def get(self, key):
        """
        :rtype: int
        """
        if key not in self.hash:
            return -1
        self.kick(self.hash[key])
        return self.hash[key].next.value
        
    def set(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: nothing
        """
        if key in self.hash:
            self.kick(self.hash[key])
            self.hash[key].next.value = value
        else:
            self.push_back(LinkedNode(key, value))
            if len(self.hash) > self.capacity:
                self.pop_front()
```