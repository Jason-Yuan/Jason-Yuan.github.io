title: "48 Majority Element III"
date: 2015-09-28 18:16:29
tags: ["LintCode", "Array", "Hash"]
categories: "LintCode"
---

# 原题
>给定一个整型数组，找到主元素，它在数组中的出现次数严格大于数组元素个数的**1/k**。

给出数组**[3,1,2,3,2,3,3,4,4,4]**，和 k =**3**，返回 3
数组中只有唯一的主元素

# 解题思路
* 大体思路同Majority Element I, Majority Element II是一样的。本题则为k-k抵消
* 维护一个hash表，当hash表中key的个数等于k的时候，所有key对应的value减一，注意**如果value减一之后为零，要删除key**
* python中dictionary相关操作
```
# get all the keys of a dictionary
dict.keys()
# delete a key in dictionary
dict.pop(key, None) # or dict.pop(key)
```

# 完整代码
```python
class Solution:
    """
    @param nums: A list of integers
    @param k: As described
    @return: The majority number
    """
    def removeKey(self, counters):
        keySet = counters.keys()
        removeList = []
        for key in keySet:
            counters[key] -= 1
            if counters[key] == 0:
                removeList.append(key)
            
        for key in removeList:
            counters.pop(key, None)
        
    
    def majorityNumber(self, nums, k):
        counters = {}
        for num in nums:
            if num not in counters:
                counters[num] = 1
            else:
                counters[num] += 1
                
            if len(counters) >= k:
                self.removeKey(counters)
      
        if len(counters) == 0:
            return -1
        
        # 从新计算剩下的数字中在原数组的出现次数
        for key in counters.keys():
            counters[key] = 0

        for num in nums:
            if num in counters:
                counters[num] += 1
            
        
        # find the max key
        maxCounter, maxKey = 0, 0
        for key, value in counters.iteritems():
            if value > maxCounter:
                maxCounter = value
                maxKey = key
        
        return maxKey
```