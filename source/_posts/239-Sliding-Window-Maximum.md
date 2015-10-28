title: "239 Sliding Window Maximum"
date: 2015-10-10 20:43:23
tags: ["LeetCode", "Heap", "Deque", "Array"]
categories: "LeetCode"
---

# 原题
>给出一个可能包含重复的整数数组，和一个大小为 k 的滑动窗口, 从左到右在数组中滑动这个窗口，找到数组中每个窗口内的最大值。

给出数组 [1,2,7,7,8], 滑动窗口大小为 k = 3. 返回 [7,7,8].
最开始，窗口的状态如下：
[`1, 2 ,7` ,7 , 8], 最大值为 7;
然后窗口向右移动一位：
[1, `2, 7, 7`, 8], 最大值为 7;
最后窗口再向右移动一位：
[1, 2, `7, 7, 8`], 最大值为 8.

# 解题思路
* 方法一：最直观的，每次从起点`i`遍历一遍窗口长度`j = i, i + k
`，找最大值，两层for循环，时间复杂度O(n * k)
* 方法二：维护一个**hash heap**，每次移动，加入右边元素O(logk)，减去左边元素O(logk)，返回maxheap中的最大值O(1)，时间复杂度为O(n * logk)
* 使用**Deque**，维护一个递减的deque，时间复杂度为O(n)
```
已知 [1， 2， 7， 3， 8， 5， 3， 2]
[1], 2进入, 2 > 1 弹出1
[2], 7进入， 7 > 2 弹出2
[7], 第一个窗口最大值为7，3进入，3 < 7
[7, 3], 第二个窗口最大值为7，8进入， 8 > 3 弹出3
[7], 8 > 7, 弹出7 
[8], 第三个窗口最大值为8
```
  为什么要用deque呢？注意这种情况
```
当前为[9, 8, 7], 6进入，6 < 7, 6进入
[9, 8, 7, 6] 但是对K=3的窗口，必须要弹出9，这步就要用到deque
```
* Python中使用deque, 和list的API类似，多了`popleft`, `appendleft`, `extendleft`
```python
import collections
d = collections.deque()
d.extend([1, 2, 3])
d.pop() # 3
d.popleft() # 1
d.append() # [2, 10]
d.appendleft() # [20, 2, 10]
d[0] # peek at leftmost item
d[-1] # peek at rightmost item
if d:
    # d is not empty
else:
    # d is empty
```

# 完整代码
```python
import collections

class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        res = []
        d = collections.deque()
        for i in range(len(nums)):
            while d and d[-1] < nums[i]:
                d.pop()
            d.append(nums[i])
            if i > k - 1 and d[0] == nums[i - k]:
                d.popleft()
            if i >= k - 1:
                res.append(d[0])
                
        return res
```