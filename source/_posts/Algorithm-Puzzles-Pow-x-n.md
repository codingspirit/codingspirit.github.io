---
title: 'Algorithm Puzzles: Pow(x, n)'
top: false
categories: 算法题解
tags:
  - Algorithm
  - Python
date: 2022-12-31 01:46:58
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Pow(x, n)
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Implement pow(x, n), which calculates x raised to the power n.

## Solution

### First came out solution

```py
class Solution:
    def myPow(self, x: float, n: int) -> float:
        return x**n
```

![](Algorithm-Puzzles-Pow-x-n/Algorithm-Puzzles-Pow-x-n-r1.gif)


### Binary Search

```py
class Solution:
    @lru_cache
    def myPow(self, x: float, n: int) -> float:
        ret = 1
        absn = abs(n)

        if absn >= 4 and absn % 2 == 0:
            half = int(n / 2)
            return self.myPow(x, half) * self.myPow(x, half)
        elif absn >= 4:
            half = int(n / 2)
            ret = self.myPow(x, half) * self.myPow(x, half)
            return ret * x if n > 0 else ret / x
        elif n > 0:
            while n > 0:
                ret = ret * x
                n = n - 1
        elif n < 0:
            while n < 0:
                ret = ret / x
                n = n + 1

        return ret
```

![](Algorithm-Puzzles-Pow-x-n/Algorithm-Puzzles-Pow-x-n-s1.png)

T.C. should be `O(n/2)`
