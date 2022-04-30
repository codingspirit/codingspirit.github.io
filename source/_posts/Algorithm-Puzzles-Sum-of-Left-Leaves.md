---
title: 'Algorithm Puzzles: Sum of Left Leaves'
top: false
tags:
  - Algorithm
  - C&#43;&#43;
date: 2022-04-30 12:37:42
categories: 算法题解
---
Algorithm Puzzles ~~everyday~~ ~~every week~~ sometimes: Sum of Left Leaves
<!--more-->
## Puzzle
Puzzle from [leetcode](https://leetcode.com):

Given the root of a binary tree, return the sum of all left leaves.

A leaf is a node with no children. A left leaf is a leaf that is the left child of another node.

## Solution

It's an easy puzzle can be resolved via dfs or bfs, here I use bfs:

```cpp
class Solution {
  public:
    int sumOfLeftLeaves(TreeNode* root) {
        int sum = 0;

        if (root == nullptr) {
            return sum;
        }

        std::queue<TreeNode*> q;
        std::queue<bool> isLeftNode;

        q.push(root);
        isLeftNode.emplace(false);

        while (!q.empty()) {
            auto cur = q.front();
            if (cur->left == nullptr && cur->right == nullptr &&
                isLeftNode.front()) {
                sum += cur->val;
            }
            q.pop();
            isLeftNode.pop();

            if (cur->left != nullptr) {
                q.push(cur->left);
                isLeftNode.emplace(true);
            }
            if (cur->right != nullptr) {
                q.push(cur->right);
                isLeftNode.emplace(false);
            }
        }

        return sum;
    }
};
```

TC should be O(n) and SC should be O(n) in worst case, where n is node number.
