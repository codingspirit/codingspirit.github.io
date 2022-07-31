---
title: 'Algorithm Puzzles: Remove Element'
top: false
categories: 算法题解
tags:
  - Algorithm
  - Python
date: 2022-07-16 19:19:03
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Remove Element
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The relative order of the elements may be changed.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

## Solution

```py
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        originalLen = nums.__len__()
        deleted = 0
        i = 0

        while i < originalLen - deleted:
            if deleted > 0:
                nums[i] = nums[i+deleted]
            if nums[i] == val:
                nums[i] = nums[i+deleted]
                deleted += 1
                i -= 1

            i += 1

        if deleted > 0:
            del nums[-deleted:]

        return nums.__len__()
```

![](Algorithm-Puzzles-Remove-Element/s1.png)
