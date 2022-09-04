---
title: 'Algorithm Puzzles: Unique Paths'
top: false
categories: 算法题解
tags:
  - Algorithm
  - Python
date: 2022-09-03 21:03:26
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Unique Paths
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 10^9.

## Solution
it's easy to resolve via dfs, but without `lru_cache` it might exceed time limit during check...

![](Algorithm-Puzzles-Unique-Paths/otter.gif)

```py
from functools import lru_cache


class Solution:
    @lru_cache()
    def uniquePaths(self, m: int, n: int) -> int:
        ret = 0
        if m == 1 and n == 1:
            ret += 1
            return ret

        if n != 1:
            # move right
            ret += self.uniquePaths(m, n - 1)
        if m != 1:
            # move down
            ret += self.uniquePaths(m - 1, n)

        return ret
```

T.C. should be `O(n^n)`

![](Algorithm-Puzzles-Unique-Paths/unique_paths_s1.png)

