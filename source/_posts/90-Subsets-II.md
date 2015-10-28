title: "90 Subsets II"
date: 2015-10-11 17:32:31
tags: ["LeetCode", "Backtracking", "DFS", "Array"]
categories: "LeetCode"
---

# 原题
>给定一个可能具有重复数字的列表，返回其所有可能的子集

如果S = [1,2,2]，一个可能的答案为：
```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```
子集中的每个元素都是非降序的
两个子集间的顺序是无关紧要的
解集中不能包含重复子集

# 解题思路
* 对于每次层，如果current == previous则跳过
```python
if i != pos and List[i] == List[i - 1]:
    continue
```

# 完整代码
```python
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if not nums:
            return []
        else:
            result.append([])
            self.dfs(sorted(nums), 0, [], result)
            return result
            
    def dfs(self, List, pos, path, result):
        for i in range(pos, len(List)):
            if i != pos and List[i] == List[i - 1]:
                continue
            else:
                result.append(path + [List[i]])
            self.dfs(List, i + 1, path + [List[i]], result)
```