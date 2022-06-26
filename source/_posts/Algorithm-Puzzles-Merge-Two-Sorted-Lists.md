---
title: 'Algorithm Puzzles: Merge Two Sorted Lists'
top: false
tags:
  - Algorithm
  - C&#43;&#43;
date: 2022-06-25 20:49:32
categories: 算法题解
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Merge Two Sorted Lists
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

## Solution

```cpp
class Solution {
 public:
  ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    if (list1 == nullptr) {
      return list2;
    }

    if (list2 == nullptr) {
      return list1;
    }

    ListNode mergedHead = ListNode();
    ListNode* mergedCur = &mergedHead;
    ListNode* list1Cur = list1;
    ListNode* list2Cur = list2;

    while (list1Cur != nullptr || list2Cur != nullptr) {
      if (list1Cur == nullptr) {
        mergedCur->next = new ListNode(list2Cur->val, list2Cur->next);
        break;
      }
      if (list2Cur == nullptr) {
        mergedCur->next = new ListNode(list1Cur->val, list1Cur->next);
        break;
      }

      if (list1Cur->val < list2Cur->val) {
        mergedCur->next = new ListNode(list1Cur->val);
        list1Cur = list1Cur->next;
      } else {
        mergedCur->next = new ListNode(list2Cur->val);
        list2Cur = list2Cur->next;
      }

      mergedCur = mergedCur->next;
    }

    return mergedHead.next;
  }
};
```

TC: O(2n)

![](Algorithm-Puzzles-Merge-Two-Sorted-Lists/s1.png)
