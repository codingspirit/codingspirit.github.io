---
title: 'Rust: Tree Structure'
top: false
tags:
  - Rust
date: 2023-04-02 10:18:12
categories: 编程相关
---

Trying to craft a Tree Structure in Rust.

<!--more-->

```rs
use std::{cell::RefCell, rc::Rc};

struct TreeNode {
    val: isize,
    left: Option<Rc<RefCell<TreeNode>>>,
    right: Option<Rc<RefCell<TreeNode>>>,
}

impl TreeNode {
    fn new(val: isize) -> Rc<RefCell<TreeNode>> {
        Rc::new(RefCell::new(TreeNode {
            val,
            left: None,
            right: None,
        }))
    }

    fn dfs(node: &Option<Rc<RefCell<TreeNode>>>) -> () {
        if let Some(node) = node {
            println!("val is {}", node.borrow().val);
            TreeNode::dfs(&node.borrow().left);
            TreeNode::dfs(&node.borrow().right);
        }
    }
}

fn main() {
    let root_node = TreeNode::new(1);

    root_node.borrow_mut().left = Some(TreeNode::new(2));
    root_node.borrow_mut().right = Some(TreeNode::new(3));

    TreeNode::dfs(&Some(root_node));
}
```

- Why `Option<T>`: Node is allowed to be `None` in tree.
- Why `Rc<T>`: In case of multiple nodes has same child node.
- Why `RefCell<T>`: Allow to mutate shared node.

If we can make sure each node has a unique owner, `Option<Box<Node<TreeNode>>>` is more suitable.

