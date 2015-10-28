title: "40 Combination Sum II"
date: 2015-09-26 16:36:09
tags: ["LeetCode", "Array", "Backtracking", "DFS"]
categories: "LeetCode"
---

# 原题
> 给出一组候选数字(C)和目标数字(T),找出C中所有的组合，使组合中数字的和为T。C中每个数字在每个组合中只能使用一次。

给出一个例子，候选数字集合为**[10,1,6,7,2,1,5] ** 和目标数字 **8
**
解集为：**[[1,7], [1,2,5], [2,6], [1,1,6]]** 

**注意**： 
* 所有的数字(包括目标数字)均为正整数。
* 元素组合(*a*1, *a*2, … , *a*k)必须是非降序(ie, *a*1 ≤ *a*2 ≤ … ≤ *a*k)。
* 解集不能包含重复的组合。 

# 解题思路
* Backtracking, DFS
* 与permutation和subset解题思路类似
* 在`combination sum I`的基础上，此题关键在于candidate中可能出现重复元素，所以使用`prev`参数每次check一下`candidates[i] == prev`的话就`continue `
因为如果candidates = [1(a), 1(b), 2, 3], target = 4
结果可能会出现 [1(a), 3], [1(b), 3] 所以如果加上判断后
```
prev = 1(a)
candidates[i] = a(b)
```
此时会`continue `

# 完整代码
```python
class Solution(object):
    def helper(self, candidates, target, path, index, result):
        if target == 0:
            result.append(path[:])
            return
        prev = -1
        for i in range(index, len(candidates)):
            if candidates[i] > target:
                break
            if prev != -1 and prev == candidates[i]:
                continue
            self.helper(candidates, target - candidates[i], path+[candidates[i]], i+1, result)
            prev = candidates[i]
            
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        result = []
        if not candidates:
            return result
        
        candidates.sort()    
        self.helper(candidates, target, [], 0, result)
        return result
```