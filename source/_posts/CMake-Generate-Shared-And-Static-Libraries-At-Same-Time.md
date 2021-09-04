---
title: 'CMake: Generate Shared And Static Libraries At Same Time'
top: false
tags:
  - CMake
  - null
date: 2021-09-04 12:28:22
categories: 编程相关
---

By creating 2 targets and set same `OUTPUT_NAME` for both targets:

```cmake
add_library(v4l2capturer-shared SHARED ${V4L2CAPTURER_SRC})
target_include_directories(v4l2capturer-shared PRIVATE ${LIBV4L2_INCLUDE_DIRS})
target_include_directories(v4l2capturer-shared PUBLIC ${V4L2CAPTURER_INC})
set_target_properties(v4l2capturer-shared PROPERTIES OUTPUT_NAME v4l2capturer)
target_link_libraries(v4l2capturer-shared PRIVATE ${LIBV4L2_LIBRARIES})

add_library(v4l2capturer-static STATIC ${V4L2CAPTURER_SRC})
target_include_directories(v4l2capturer-static PRIVATE ${LIBV4L2_INCLUDE_DIRS})
target_include_directories(v4l2capturer-static PUBLIC ${V4L2CAPTURER_INC})
set_target_properties(v4l2capturer-static PROPERTIES OUTPUT_NAME v4l2capturer)
target_link_libraries(v4l2capturer-static PRIVATE ${LIBV4L2_LIBRARIES})
```
