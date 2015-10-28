title: "128 Longest Consecutive Sequence"
date: 2015-09-27 23:40:33
tags: ["LeetCode", "Hash"]
categories: "LeetCode"
---

# 原题
>给定一个未排序的整数数组，找出最长连续序列的长度。

给出数组**[100, 4, 200, 1, 3, 2]**，这个最长的连续序列是** [1, 2, 3, 4]**，返回所求长度 4
要求你的算法复杂度为O(n)

# 解题思路
* 从第一个数字100开始看我们希望知道100+1和100-1在不在数组中。对于这类会员查询的问题，首先想到hash表。O(1)时间复杂度查询。
* 第一次遍历建立hash[100]=1, hash[4]=1 ......
* 第二次遍历查询100+1和100-1，注意，比如通过4找到了1，2，3我们就可以把1，2，3删掉了，所以查询的过程中一边找一边删


# 完整代码
```python
class Solution(object):
    def longestConsecutive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        map = {}
        
        for e in nums:
            if e not in map:
                map[e] = 1

        res = 0        
        for e in nums:
            if map[e] != -1:
                temp = map[e]
                lg = sm = e
                while lg + 1 in map and map[lg + 1] != -1:
                    temp += map[lg + 1]
                    map[lg + 1] = -1
                    lg += 1
                while sm - 1 in map and map[sm - 1] != -1:
                    temp += map[sm - 1]
                    map[sm - 1] = -1
                    sm -= 1
            if temp > res:
                res = temp
                
        return res
```