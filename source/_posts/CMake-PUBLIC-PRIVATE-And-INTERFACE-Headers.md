---
title: 'CMake: PUBLIC PRIVATE And INTERFACE Headers'
top: false
tags:
  - CMake
date: 2021-09-04 12:41:17
categories: 编程相关
---

...

<!--more-->

There are 3 keywords in CMake `target_include_directories`:

  - `PRIVATE` : header files with this keyword will be add to current target's include directories. We use this if header will be used by current target only.
  - `INTERFACE` : header files with this keyword will not be add to current target's include directories, but will be treated as public headers of this library. We use it when headers are external only, current target will not use those during compiling.
  - `PUBLIC` : header files with this keyword will be performed with both `PRIVATE` and `INTERFACE`. In this case, both current target and external public header lists will be modified.

To combine with `install`, `PUBLIC` and `INTERFACE` headers should be installed to user. Taking `PUBLIC` as an example:

```cmake
set(INCS include/V4l2Capturer.h)
target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC include)
set_target_properties(${CMAKE_PROJECT_NAME} PROPERTIES PUBLIC_HEADER ${INCS})

install(TARGETS ${CMAKE_PROJECT_NAME}
        LIBRARY DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_FULL_INCLUDEDIR})
```

We need to manually set `PUBLIC_HEADER` property for public headers, and install it with keyword `PUBLIC_HEADER`.
