title: "46 Permutations"
date: 2015-10-11 17:49:44
tags: ["LeetCode", "Array", "Backtracking", "DFS", "Permutation"]
categories: "LeetCode"
---

# 原题
>给定一个数字列表，返回其所有可能的排列。

给出一个列表[1,2,3]，其全排列为：
```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

# 解题思路
* 与subset题相似，只不过当`len(path) == len(list)`的时候才会将path加入到结果
* 对于每一层，遍历list中的元素，把不在path中的元素加入path

# 完整代码
```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if nums is None:
            return []
        result = []
        self.helper(nums, [], result)
        return result
        
    def helper(self, List, path, result):
        if len(path) == len(List):
            result.append(path[:])
            return
        
        for i in range(len(List)):
            if List[i] in path:
                continue
            path.append(List[i])
            self.helper(List, path, result)
            path.pop()
```