---
title: 'Algorithm Puzzles: Rotate Image'
top: false
categories: 算法题解
tags:
  - Algorithm
  - C&#43;&#43;
date: 2022-11-06 01:21:02
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Rotate Image
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

## Solution

```cpp
class Solution {
 public:
  // m[x][y] -> m[y][n-1-x]
  void rotate(std::vector<std::vector<int>>& matrix) {
    n = matrix.size();
    int x = 0, y = 0;
    int totalNum = n * n;
    int rotatedNum = 0;
    int end = n - 1;

    for (int x = 0; x < end; ++x) {
      for (int y = x; y < end; ++y) {
        rotateFour(matrix, x, y);
        rotatedNum += 4;
        if (rotatedNum >= totalNum) {
          return;
        }
      }
      end--;
    }
  }

 private:
  int n;

  void rotateFour(std::vector<std::vector<int>>& matrix, const int x,
                  const int y) const {
    int tmp = matrix[x][y];
    matrix[x][y] = matrix[n - 1 - y][x];
    matrix[n - 1 - y][x] = matrix[n - 1 - x][n - 1 - y];
    matrix[n - 1 - x][n - 1 - y] = matrix[y][n - 1 - x];
    matrix[y][n - 1 - x] = tmp;
  }
};
```

![](Algorithm-Puzzles-Rotate-Image/Algorithm-Puzzles-Rotate-Image-s1.png)

T.C: O(n*n)
