---
title: 'Algorithm Puzzles: Implement strStr()'
top: false
categories: 算法题解
tags:
  - Algorithm
  - C&#43;&#43;
date: 2022-07-01 18:44:05
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Implement strStr()
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Implement `strStr()`.

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

## Solution

```cpp
class Solution {
 public:
  int strStr(const std::string& haystack, const std::string& needle) {
    if (needle.empty()) {
      return 0;
    }

    if (needle.size() > haystack.size()) {
      return -1;
    }

    auto needleIt = needle.begin();
    int index = -1;

    for (auto haystackIt = haystack.begin(); haystackIt != haystack.end();
         ++haystackIt) {
      if (*needleIt == *haystackIt) {
        if (index < 0) {
          index = std::distance(haystack.begin(), haystackIt);
        }
        needleIt++;
        if (needleIt == needle.end()) {
          return index;
        }
      } else if (index >= 0) {
        haystackIt = haystack.begin() + index;
        index = -1;
        needleIt = needle.begin();
      }
    }

    return -1;
  }
};
```

![](Algorithm-Puzzles-Implement-strStr/s1.png)
