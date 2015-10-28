title: "127 Word Ladder I"
date: 2015-09-26 16:39:29
tags: ["LeetCode", "BFS", "Level Order Traversal"]
categories: "LeetCode"
---

### 原题
>给出两个单词（start和end）和一个字典，找到从start到end的最短转换序列
比如：
1.每次只能改变一个字母。
2.变换过程中的中间单词必须在字典中出现。

start = **"hit"**
end = **"cog"**
dict = ** ["hot","dot","dog","lot","log"]**
一个最短的变换序列是 
**"hit"**->**"hot" **->** "dot"**->** "dog"**->**"cog"** 返回它的长度 5

**注意**:
* 如果没有转换序列则返回0。
* 所有单词具有相同的长度。
* 所有单词都只包含小写字母。

# 解题思路
* 无向图求最短路 - BFS 中的**level order**
```
# 即每次count一下这一层的size()，然后遍历这一层
size = q.qsize()
for i in range(size):
```
* 题目中如果提到单词，要考虑到单词长度一般是有限的，进行优化
* 如何找通过一次改变可以形成的单词在不在字典中，假设单词长度为L，变量单词，每个位置可以有26种变换，所以O(L*26)，再check是否存在在字典中O(1)复杂度（更精确，应该是O(L)的复杂度）
* ord('a') => 97 / chr(97) => a
* Java中的`HashSet<xxx>`可以用Python中的`Set()`
* 注意Python中string是immutable
```
import string
aToz = string.ascii_lowercase
# 'str' object does not support item assignment
newWord[i] = 'a' 
# 所以，换种方式
newWord = current[:i] + 'a' + current[i+1:]
```

# 完整代码
```python
import Queue
import string
class Solution(object):
    def getNextWord(self, word, dict):
        aToz = string.ascii_lowercase
        res = []
        for char in aToz:
            for j in range(len(word)):
                if word[j] == char:
                    continue
                newWord = word[:j] + char + word[j+1:]
                if newWord in dict:
                    res.append(newWord)
        return res
        
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: Set[str]
        :rtype: int
        """
        if not wordList:
            return 0
            
        q = Queue.Queue()
        visited = set()
        q.put(beginWord)
        visited.add(beginWord)
        length = 1
        while not q.empty():
            length += 1
            size = q.qsize()
            for i in range(size):
                word = q.get()
                for nextWord in self.getNextWord(word, wordList):
                    if nextWord in visited:
                        continue
                    if nextWord == endWord:
                        return length
                    q.put(nextWord)
                    visited.add(nextWord)
        return 0
```