title: "140 Fast Power"
date: 2015-10-03 03:36:08
tags: ["LintCode", "Divide and Conquer", "Math"]
categories: "LintCode"
---

# 原题
>计算**a^n % b**，其中a，b和n都是32位的整数。

例如 2^31 % 3 = 2
例如 100^1000 % 1000 = 0

# 解题思路
* 本题主要利用了整数求模的一些特性
```
(a * b) % p = ((a % p) * (b % p)) % p
```
* 所以求解n次方可以转化为n/2次方....
* 典型divide & conquer
* 需要注意对n=0, n=1, n<0情况的分析

# 完整代码
```python
class Solution:
    """
    @param a, b, n: 32bit integers
    @return: An integer
    """
    def fastPower(self, a, b, n):
        if n == 1:
            return a % b
        elif n == 0:
            return 1 % b
        elif n < 0:
            return -1
        
        product = self.fastPower(a, b, n / 2)
        product = (product * product) % b
        if n % 2 == 1:
            product = (product * a) % b
        
        return product
```