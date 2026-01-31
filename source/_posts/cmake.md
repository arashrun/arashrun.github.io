---
title: cmake
date: 2023-03-13 10:25:38
categories:
- 工具
tags:
- cmake
---


## 基本概念

target:代表可执行程序或库（elf，so）
    add_executable
    add_library
    add_custom_command
project:
command:
target properties:
`CMakeCache.txt` : cmake的配置缓存，删除该文件与删除build目录作用相同


1. cmake构建系统是由一系列target组成,cmake就是围绕target进行展开的。


## 预设-preset

`CMakePresets.json` :CMakePresets.json 是 CMake 3.19+ 引入的一个配置文件，旨在将 CMake 的配置选项（如工具链、生成器、缓存变量等）标准化、版本化并保存到项目中，从而消除对冗长命令行参数的依赖，实现一键配置和跨团队/跨平台的一致构建。

基本结构：
```json
{
  "version": 6,
  "cmakeMinimumRequired": {
    "major": 3,
    "minor": 23,
    "patch": 0
  },
  "configurePresets": [
    {
      "name": "linux-debug",
      "displayName": "Linux Debug",
      "description": "使用主机 GCC 进行调试构建",
      "generator": "Unix Makefiles",
      "binaryDir": "${sourceDir}/build/debug",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug",
        "CMAKE_CXX_FLAGS": "-Wall -Wextra"
      }
    },
    {
      "name": "arm-cross-release",
      "displayName": "ARM Cross-Compile Release",
      "description": "交叉编译 ARM 的发布版本",
      "generator": "Unix Makefiles",
      "binaryDir": "${sourceDir}/build/arm-release",
      "toolchainFile": "${sourceDir}/toolchains/arm-linux-gnueabihf.cmake",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Release"
      }
    },
    {
      "name": "android-arm64",
      "displayName": "Android ARM64",
      "description": "使用 NDK 交叉编译 Android",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/android",
      "toolchainFile": "$env{ANDROID_NDK}/build/cmake/android.toolchain.cmake",
      "cacheVariables": {
        "ANDROID_ABI": "arm64-v8a",
        "ANDROID_PLATFORM": "android-24"
      }
    }
  ],
  "buildPresets": [
    {
      "name": "debug-build",
      "configurePreset": "linux-debug",
      "jobs": 8
    },
    {
      "name": "arm-build",
      "configurePreset": "arm-cross-release",
      "targets": ["all", "install"]
    }
  ]
}
```

继承与复用：预设支持继承，避免重复配置：

```json
{
  "configurePresets": [
    {
      "name": "base",
      "hidden": true,
      "generator": "Ninja",
      "cacheVariables": { "CMAKE_EXPORT_COMPILE_COMMANDS": true }
    },
    {
      "name": "arm-base",
      "inherits": "base",
      "toolchainFile": "${sourceDir}/toolchains/arm.cmake"
    },
    {
      "name": "arm-debug",
      "inherits": ["arm-base", "debug-opts"]
    }
  ]
}
```

使用预设

```bash
# 列出所有可用的配置预设
cmake --list-presets

# 使用特定预设进行配置（替代 cmake -B build -D...）
cmake --preset=arm-cross-release

# 使用构建预设进行构建（替代 cmake --build build --target install -j8）
cmake --build --preset=arm-build
```


