# cmakelists.txt

## 语法

cmake_minimum_required(VERSION 2.8)	//cmake最低版本要求  
project(name)				//项目名称  
add_executable(name file1 file2)	//通过多个file文件构建name可执行文件  
set(var v1 v2)				//设置变量var为v1 v2  
set(CMAKE_CXX_FLAGS "-std=c++11")
include_directories(path1 path2)	//头文件路径  
link_directories(path1)			//库文件路径  
target_link_libraries(name ws2_32.lib)	//具体链接的库  

## cmake项目的构建流程  

1. 进入项目目录，创建一个build文件夹用于存放构建过程的中间文件  
2. 进入build文件夹，cmake -G "xxx" ..  
3. 根据xxx不同编译系统。cmake会生成不同项目构建文件。windows下会生成sln解决方案，linux下会生成makefile，当然windows也可以生成nmake makefile  
4. 使用cmake构建出了不同编译环境的makefile文件，或sln。然后通过make（unix-like）工具，编译构建。windows下运行nmake。  


## 基本概念

target:代表可执行程序或库（elf，so）
    add_executable
    add_library
    add_custom_command
project:
command:
target properties:

## 基本概念

1. cmake构建系统是由一系列target组成,cmake就是围绕target进行展开的。
