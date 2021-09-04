---
title: 'CMake: fetch and build dependencies'
top: false
tags:
  - CMake
date: 2021-05-05 17:13:49
categories: 编程相关
---

Using cmake to fetch and build instead of using git submodule.

<!--more-->

## FetchContent

```cmake
include(FetchContent)
set(FETCHCONTENT_QUIET FALSE)
FetchContent_Declare(log4cplus
  GIT_REPOSITORY    https://github.com/log4cplus/log4cplus.git
  GIT_TAG           REL_2_0_6
  GIT_PROGRESS      TRUE
)
FetchContent_MakeAvailable(log4cplus)
add_dependencies(log4cplus)
target_link_libraries(${PROJECT_NAME} PRIVATE log4cplus)
```

## ExternalProject

```cmake
include(ExternalProject)

function (build_external_project TARGET ARGS GIT_REPOSITORY GIT_TAG)

    set(trigger_build_dir ${CMAKE_BINARY_DIR}/force_${TARGET})

    #mktemp dir in build tree
    file(MAKE_DIRECTORY ${trigger_build_dir} ${trigger_build_dir}/build)

    #generate false dependency project
    set(CMAKE_LIST_CONTENT "
        cmake_minimum_required(VERSION 3.14)
        include(ExternalProject)
        ExternalProject_add(${TARGET}
          GIT_REPOSITORY  ${GIT_REPOSITORY}
          GIT_TAG         ${GIT_TAG}
          CMAKE_ARGS      ${ARGS}
          TEST_COMMAND    \"\"
        )
        add_custom_target(trigger_${TARGET})
        add_dependencies(trigger_${TARGET} ${TARGET})
    ")
    message(STATUS "CMAKE_ARGS is ${CMAKE_ARGS}")
    file(WRITE ${trigger_build_dir}/CMakeLists.txt "${CMAKE_LIST_CONTENT}")

    execute_process(COMMAND ${CMAKE_COMMAND} ..
        WORKING_DIRECTORY ${trigger_build_dir}/build
    )
    execute_process(COMMAND ${CMAKE_COMMAND} --build .
        WORKING_DIRECTORY ${trigger_build_dir}/build
    )

endfunction()

build_external_project(log4cplus
  "-DCMAKE_INSTALL_PREFIX=${MY_INSTALL_DIR} -DLOG4CPLUS_BUILD_TESTING=OFF -DBUILD_SHARED_LIBS=OFF"
  https://github.com/log4cplus/log4cplus.git
  REL_2_0_6
)
```
