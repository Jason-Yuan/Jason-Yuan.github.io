title: "78 Subsets"
date: 2015-10-11 16:50:17
tags: ["LeetCode", "Backtracking", "DFS", "Array"]
categories: "LeetCode"
---

# 原题
>给定一个含不同整数的集合，返回其所有的子集

如果 S = [1,2,3]，有如下的解：
```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
子集中的元素排列必须是非降序的，解集必须不包含重复的子集

# 解题思路
* Backtracking, DFS
* 首先，数组要排序，在第n层，加入一个元素进入n+1层，删除刚刚加入的元素，加入第n层的第二个元素......
```
                                   [ ]
                                /   |   \
                             [1]   [2]   [3]
                           /  |     |
                    [1, 2] [1, 3] [2, 3]
                      /
                [1, 2, 3]
```

# 完整代码
```python
# 方法一
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if nums is None:
            return []
        res = [[]]
        self.dfs(sorted(nums), 0, [], res)
        return res

    def dfs(self, nums, index, path, res):
        for i in xrange(index, len(nums)):
            res.append(path + [nums[i]])
            path.append(nums[i])
            self.dfs(nums, i+1, path, res)
            path.pop()

# 方法二
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if nums is None:
            return []
        res = [[]]
        self.dfs(sorted(nums), 0, [], res)
        return res

    def dfs(self, nums, index, path, res):
        for i in xrange(index, len(nums)):
            res.append(path + [nums[i]])
            self.dfs(nums, i+1, path+[nums[i]], res)

# 方法三
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort()
        result = [[]]
        for x in nums:
            with_x = []
            for s in result:
                with_x.append(s + [x])
            result = result + with_x
	return result
```