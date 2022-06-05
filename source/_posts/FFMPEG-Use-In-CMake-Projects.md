---
title: 'FFMPEG: Use In CMake Projects'
top: false
tags:
  - CMake
date: 2022-06-04 23:25:11
categories: 编程相关
---

To add FFMPEG(libav) into CMake project, we can use `pkgconfig`:

```cmake
find_package(PkgConfig REQUIRED)
pkg_check_modules(FFMPEG REQUIRED IMPORTED_TARGET
    libavdevice
    libavformat
    libavcodec
    libavutil
    libavfilter
    libswresample
    libswscale
)

target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE PkgConfig::FFMPEG)
target_link_libraries(${PROJECT_NAME} PRIVATE PkgConfig::FFMPEG)
```

> Maybe not all libs are required by your project. Remove unnecessary libs if you want.
