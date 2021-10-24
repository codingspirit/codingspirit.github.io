---
title: 'GCC: --wrap option'
top: false
tags:
  - C
  - C&#43;&#43;
date: 2021-09-21 16:36:26
categories: 编程相关
---

GNU linker provides an `--wrap` option which can help developer replace symbols easily.

<!--more-->

In `man ld`:

```
--wrap=symbol
    Use a wrapper function for symbol.  Any undefined reference to symbol will be resolved to
    "__wrap_symbol".  Any undefined reference to "__real_symbol" will be resolved to symbol.

    This can be used to provide a wrapper for a system function.  The wrapper function should
    be called "__wrap_symbol".  If it wishes to call the system function, it should call
    "__real_symbol".

```

One of user scenario is when developer is trying to analyze heap usage with customized `malloc` and `free`(i.e. using memory pool):


```c
void * __wrap_malloc (size_t size)
{
  printf ("malloc called with %zu\n", size);
  return memory_pool_malloc(size);
}
//Same for free, realloc and calloc
```

Then link with option `--wrap,malloc --wrap,free --wrap,realloc --wrap,calloc`. If user want to mix customized `malloc` and system `malloc`, they can call `__real_malloc` when they want to call the system `malloc`.

