title: "39 Combination Sum I"
date: 2015-09-26 15:58:17
tags: ["LeetCode", "DFS", "Sum", "Array", "Backtracking"]
categories: "LeetCode"
---

# 原题
>给出一组候选数字(C)和目标数字(T),找到C中所有的组合，使找出的数字和为T。C中的数字可以无限制重复被选取。

例如,给出候选数组**[2,3,6,7]**和目标数字7
所求的解为：**[7]** 和 **[2,2,3]** 

**注意**： 
* 所有的数字(包括目标数字)均为正整数。
* 元素组合(*a*1, *a*2, … , *a*k)必须是非降序(ie, *a*1 ≤ *a*2 ≤ … ≤ *a*k)。
* 解集不能包含重复的组合。 

# 解题思路
* backtracking 在代码的实现上可以在使用`加了再还原`的方式，例如：
```
path.append(candidates[i])
self.helper(candidates, target - candidates[i], path, i, result)
path.pop()
```
 也可以直接把添加的步骤直接通过参数传递的方式，例如：
```
self.helper(candidates, target - candidates[i], path + [candidates[i]], i, result)
```
* 与permutation和subset的思考方式类似，DFS
* 每一次向下搜索时，起始位置都和上一次相同，因为可以取相同元素不止一次，即每次向下传入的`index`
* For循环每次从`index`开始，避免返回到之前的元素

# 完整代码
```python
class Solution(object):
    def combinationSum(self, candidates, target):
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
        
    def helper(self, candidates, target, path, index, result):
        if target == 0:
            result.append(path[:])
            return
        
        for i in range(index, len(candidates)):
            if candidates[i] > target:
                break
            self.helper(candidates, target - candidates[i], path + [candidates[i]], i, result)
```