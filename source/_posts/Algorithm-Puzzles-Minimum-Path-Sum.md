---
title: 'Algorithm Puzzles: Minimum Path Sum'
top: false
categories: 算法题解
tags:
  - Algorithm
  - C&#43;&#43;
date: 2023-05-14 14:01:07
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: {{ title }}
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

## Solution

```cpp
class Solution final {
 public:
  int minPathSum(const std::vector<std::vector<int>>& grid) const {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);

    const int height = grid.size();
    const int width = grid[0].size();

    std::vector<std::vector<int>> dp(height,
                                     std::vector<int>(width, __INT_MAX__));

    dp[0][0] = grid[0][0];

    for (int y = 0; y < height; ++y) {
      int ly = std::max(y - 1, 0);
      for (int x = 0; x < width; ++x) {
        int lx = std::max(x - 1, 0);
        if (dp[y][x] == __INT_MAX__) {
          dp[y][x] = std::min(dp[ly][x], dp[y][lx]) + grid[y][x];
        }
      }
    }

    return dp[height - 1][width - 1];
  }
};
```

![](Algorithm-Puzzles-Minimum-Path-Sum/Algorithm-Puzzles-Minimum-Path-Sum-s1.png)
