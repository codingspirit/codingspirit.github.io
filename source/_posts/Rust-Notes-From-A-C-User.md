---
title: 'Rust Notes: From A C++ User'
top: false
tags:
  - Rust
date: 2023-04-20 21:19:22
categories: 编程相关
---

Learn Rust from C++ programmer's perspective

<!--more-->

## loop

If we have a `vec`:

```rs
let vec = vec![1, 2, 3];
```

### for range

```rs
for i in 0..vec.len() {
    println!("{}, {}", i, match_test(&vec[i]));
}
```

### for each

```rs
// type of `item` is &u64
for item in &vec {
    println!("{}", item);
}
```

If `vec` is not used as reference here, there will be an implicit move:

```rs
//`vec` moved due to this implicit call to `.into_iter()`
for item in vec {
    println!("{}", item);
}
```

Or we can use iterator, which is not exactly same as we do in C++:

```rs
// type of `item` is &u64 
for item in vec.iter() {
    println!("{}", item)
}
```

Or use `enumerate`, same as we do in python:

```rs
for (index, item) in vec.iter().enumerate() {
    println!("{}, {}", index, item);
}
```

### dead loop

Same as `while(true)`:

```rs
let mut i = 0;
loop {
    if i > 2 {
        break;
    }
    println!("{}", vec[i]);
    i += 1;
}
```

## Heap

To allocate memory on heap, use `Box<T>`:

```rs
let x = Box::<usize>::new(20);
```

`Box<T>` is very similar to `std::unique_ptr<T>` in C++. `const auto x = std::make_unique<const int>(123)` can be written as `let x = Box::new(123)` in Rust.


