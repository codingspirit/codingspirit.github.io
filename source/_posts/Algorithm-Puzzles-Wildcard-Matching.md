---
title: 'Algorithm Puzzles: Wildcard Matching'
top: false
tags:
  - Algorithm
  - Python
date: 2022-04-05 15:12:07
categories: 算法题解
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Wildcard Matching
<!--more-->

## Puzzle

Puzzle from [leetcode](https://leetcode.com):

Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

    '?' Matches any single character.
    '*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

## Solving

Using backtrace:
```py
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        if not s and not p:
            return True

        si, pi = 0, 0
        sl, pl = len(s), len(p)
        starIndex, siLast = -1, -1

        while si < sl:
            if pi < pl and (s[si] == p[pi] or p[pi] == '?'):
                si += 1
                pi += 1
            # Check if * can be used as empty string
            elif pi < pl and p[pi] == '*':
                siLast = si
                starIndex = pi
                pi += 1
            # Not match but have *. Try s[siLast+1] and p[starIndex+1]
            elif starIndex != -1:
                siLast += 1
                si = siLast
                pi = starIndex + 1
            else:  # Not match and have no *
                return False

        while pi < pl and p[pi] == '*':
            pi += 1

        return pi == pl
```

![](Algorithm-Puzzles-Wildcard-Matching/s1.png)
