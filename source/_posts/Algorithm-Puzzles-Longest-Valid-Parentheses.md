---
title: 'Algorithm Puzzles: Longest Valid Parentheses'
top: false
tags:
  - Algorithm
  - C&#43;&#43;
date: 2021-11-28 14:10:18
categories: 算法题解
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Longest Substring Without Repeating Characters
<!--more-->

## Puzzle

Puzzle from [leetcode](https://leetcode.com):

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

Example 1:

Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".

Example 2:

Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".

Example 3:

Input: s = ""
Output: 0


## Solution

```cpp
class Solution {
 public:
  int longestValidParentheses(const std::string& s) {
    std::stack<int> stack;
    int res = 0;

    stack.push(-1);

    for (int i = 0; i < s.length(); ++i) {
      if (s[i] == '(') {
        stack.push(i);
      } else {
        stack.pop();
        if (stack.empty()) {
          stack.push(i);
        } else {
          res = std::max(res, i - stack.top());
        }
      }
    }
    return res;
  }
};
```

![](Algorithm-Puzzles-Longest-Valid-Parentheses/s1.png)
