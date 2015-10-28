title: "143 Sort Color II"
date: 2015-10-02 18:54:14
tags: ["LintCode", "Bucket Sort", "Array"]
categories: "LintCode"
---

# 原题
> 给定一个有n个对象（包括k种不同的颜色，并按照1到k进行编号）的数组，将对象进行分类使相同颜色的对象相邻，并按照1,2，...k的顺序进行排序。

给出colors=[3, 2, 2, 1, 4]，k=4, 你的代码应该在原地操作使得数组变成[1, 2, 2, 3, 4]

# 解题思路
* 方法1: Count sort, 扫两遍，需要O(k)的空间
* 这道题原数组还要**重复利用作为bucket数组**!!! 首先for循环遍历一遍原来的数组，如果扫到a[i]，首先检查a[a[i]]是否为正数，如果是把a[a[i]]移动a[i]存放起来，然后把a[a[i]]记为-1(表示该位置是一个计数器，计1)。 如果a[a[i]]是负数，那么说明这一个地方曾经已经计数了，那么把a[a[i]]计数减一，并把color[i] 设置为0 （表示此处已经计算过），然后重复向下遍历下一个数，这样遍历原数组所有的元素过后，数组a里面实际上存储的每种颜色的计数，然后我们倒着再输出每种颜色就可以得到我们排序后的数组。
例子(按以上步骤运算)：
3 2 2 1 4
2 2 -1 1 4
2 -1 -1 1 4
0 -2 -1 1 4
-1 -2 -1 0 4
-1 -2 -1 -1 0

# 完整代码
```python
class Solution:
    """
    @param colors: A list of integer
    @param k: An integer
    @return: nothing
    """
    def sortColors2(self, colors, k):
        if not colors:
            return
        
        for i in range(len(colors)):
            while colors[i] > 0:
                num = colors[i]
                if colors[num - 1] > 0:
                    colors[i] = colors[num - 1]
                    colors[num - 1] = -1
                elif colors[num - 1] <= 0:
                    colors[num - 1] -= 1
                    colors[i] = 0
        
        index = len(colors) - 1
        for i in range(k - 1, -1, -1):
            temp = colors[i]
            while temp < 0:
                colors[index] = i + 1
                temp += 1
                index -= 1    
```