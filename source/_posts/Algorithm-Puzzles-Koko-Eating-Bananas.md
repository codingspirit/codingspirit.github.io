---
title: 'Algorithm Puzzles: Koko Eating Bananas'
top: false
categories: 算法题解
tags:
  - Algorithm
  - C&#43;&#43;
date: 2022-11-12 20:03:57
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Koko Eating Bananas
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer k such that she can eat all the bananas within h hours.

## Solution

Binary search:

```cpp
class Solution {
 public:
  int minEatingSpeed(const std::vector<int>& piles, const int h) {
    const int max = *std::max_element(piles.begin(), piles.end());
    int k = 1;
    int lastValidK = max;
    int tmpK = 0;
    int binarySearchS = 1;
    int binarySearchE = max;

    while (k >= binarySearchS && k <= binarySearchE) {
      if (!isEatingAble(piles, k, h)) {
        binarySearchS = k;
        tmpK = (binarySearchS + binarySearchE) / 2;
        k = tmpK > k ? tmpK : k + 1;
      } else {
        if (k >= lastValidK) {
          return lastValidK;
        }
        binarySearchE = k;
        tmpK = (binarySearchS + binarySearchE) / 2;
        lastValidK = k;
        k = tmpK < k ? tmpK : k - 1;
      }
    }

    if (k < binarySearchS) {
      return binarySearchS;
    }

    return k;
  }

 private:
  bool isEatingAble(const std::vector<int>& piles, const int k,
                    const int max) const {
    size_t estimateHours = 0;
    for (auto iter = piles.begin(); iter != piles.end(); ++iter) {
      estimateHours += (*iter) % k == 0 ? (*iter) / k : (*iter) / k + 1;
      if (estimateHours > max) {
        return false;
      }
    }

    return true;
  }
};
```

![](Algorithm-Puzzles-Koko-Eating-Bananas/Algorithm-Puzzles-Koko-Eating-Bananas-s1.png)

T.C.: O(N+logN)
S.C.: O(N)
> T.C. of std::max_element is O(N) and binary search has O(logN)
