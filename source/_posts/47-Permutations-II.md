title: "47 Permutations II"
date: 2015-10-11 18:04:00
tags: ["LeetCode", "Array", "Backtracking", "DFS", "Permutation"]
categories: "LeetCode"
---

# 原题
>给出一个具有重复数字的列表，找出列表所有不同的排列

给出列表[1,2,2]，不同的排列有：
```
[
    [1,2,2],
    [2,1,2],
    [2,2,1]
]
```
# 解题思路
* 首先数组排序，跳出recursion条件为`len(path) == len(num)`
* 设置一个flag数组，来判定一个元素是否被访问过
* 如果`previous`没有被访问，而且`current == previous`，则会出现重复，所以跳过

# 完整代码
```python
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        result = []
        flags = [False] * len(nums)
        self.permuteForReal(sorted(nums), result, flags, [])
        return result

    def permuteForReal(self, num, output, flags, path):
        if len(path) == len(num):
            output.append(path[:])
            return
         
        for i in range(len(num)):
            if not flags[i]:
                if i != 0 and not flags[i-1] and num[i] == num[i-1]:
                    continue
                path.append(num[i])
                flags[i] = True
                self.permuteForReal(num, output, flags, path)
                path.pop()
                flags[i] = False
```