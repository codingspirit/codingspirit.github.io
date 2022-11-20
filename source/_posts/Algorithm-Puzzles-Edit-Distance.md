---
title: 'Algorithm Puzzles: Edit Distance'
top: false
categories: 算法题解
tags:
  - Algorithm
  - Python
date: 2022-11-19 18:50:23
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Edit Distance
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character

## Solution

```py
class Solution:
    dp = {}
    @lru_cache
    def minDistance(self, word1: str, word2: str) -> int:
        if not word1:
            return len(word2)
        if not word2:
            return len(word1)
        if word1[0] == word2[0]:
            return self.minDistance(word1[1:], word2[1:])
        if (word1, word2) not in self.dp:
            # insert makes word1[n-1] == word2[n]
            opInsert = self.minDistance(word1, word2[1:]) + 1
            # delete makes word1[n+1] == word2[n]
            opDelete = self.minDistance(word1[1:], word2) + 1
            # replace makes word1[n] == word2[n]
            opReplace = self.minDistance(word1[1:], word2[1:]) + 1
            self.dp[(word1, word2)] = min(opInsert, opDelete, opReplace)
            
        return self.dp[(word1, word2)]
```

![](Algorithm-Puzzles-Edit-Distance/Algorithm-Puzzles-Edit-Distance-s1.png)

T.C. should be `O(len(word1) * len(word2))`
