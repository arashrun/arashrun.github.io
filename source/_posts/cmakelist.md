
---
title: cmakelists
date: 2021-07-21 18:01:12
categories:
- 工具
tags:
- cmake
---




## 语法

```cmakelists
cmake_minimum_required(VERSION 2.8)	//cmake最低版本要求  
project(name)				//项目名称  
add_executable(name file1 file2)	//通过多个file文件构建name可执行文件  
set(var v1 v2)				//设置变量var为v1 v2  
set(CMAKE_CXX_FLAGS "-std=c++11")
include_directories(path1 path2)	//头文件路径  
link_directories(path1)			//库文件路径  
target_link_libraries(name ws2_32.lib)	//具体链接的库  

```

