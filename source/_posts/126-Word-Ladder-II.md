title: "126 Word Ladder II"
date: 2015-09-26 16:41:39
tags: ["LeetCode", "Array", "String", "Backtracking", "BFS", "DFS"]
categories: "LeetCode"
---

# 原题
>给出两个单词（start和end）和一个字典，找出所有从start到end的最短转换序列
比如：
1.每次只能改变一个字母。
2.变换过程中的中间单词必须在字典中出现。

start = **"hit"**
end = **"cog"**
dict = **["hot","dot","dog","lot","log"]**

返回
[
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]

**注意**
* 所有单词具有相同的长度。
* 所有单词都只包含小写字母。

# 解题思路
* 首先，善用一个条件，所有的单词**长度相同**
* 建立一个index，作用就是把去掉某一个位置的字母后，剩余部分相同的放入一个桶中。为了优化时间复杂度。比如
```
"abcd" -> (1, bcd) <- "fbcd"
```
* BFS一遍，确定为一个单词在图中的第几层
* DFS一遍，找到每一个可行的路线。此时BFS的作用就是从第n层只能往n+1层走，以保证最短路径
* 为什么使用`dict.get(key)`而不是`dict[key]`？
```
# 可以设定一个default value, 如果key不存在，就建立相应的key-value pair
a = dictionary.get("bogus", None)   # return None
a = dictionary["bogus"]             # raise KeyError
```

# 完整代码
```python
import Queue

class Solution(object):
    def buildIndex(self, length, dict):
        indexes = []
        for i in range(length):
            index = {}
            for word in dict:
                entry = word[:i] + word[i + 1:]
                words = index.get(entry, [])
                words.append(word)
                index[entry] = words
            indexes.append(index)
        return indexes
     
    def getNextWord(self, word):
        res = []
        for i in range(len(word)):
            entry = word[:i] + word[i + 1:]
            if entry in self.indexes[i]:
                for nextWord in self.indexes[i][entry]:
                    if nextWord != word:
                        res.append(nextWord)
        return res
        
    def BFS(self, start, end):
        self.distance = {}
        self.distance[start] = 0
        q = Queue.Queue()
        q.put(start)
        while not q.empty():
            head = q.get()
            for nextWord in self.getNextWord(head):
                if nextWord not in self.distance:
                    self.distance[nextWord] = self.distance[head] + 1
                    q.put(nextWord)
    
    def DFS(self, current, target, path):
        if current == target:
            self.result.append(path[:])
            return
        
        for word in self.getNextWord(current):
            if self.distance[word] - 1 == self.distance[current]:
                self.DFS(word, target, path + [word])
        
    def findLadders(self, beginWord, endWord, wordlist):
        """
        :type beginWord: str
        :type endWord: str
        :type wordlist: Set[str]
        :rtype: List[List[int]]
        """
        if beginWord is None or endWord is None or len(beginWord) != len(endWord):
            return []
            
        if beginWord not in wordlist or endWord not in wordlist:
            return []
            
        self.indexes = self.buildIndex(len(beginWord), wordlist)
        self.BFS(beginWord, endWord)
        
        self.result = []
        if beginWord in self.distance:
            self.DFS(beginWord, endWord, [beginWord])
            
        return self.result
```