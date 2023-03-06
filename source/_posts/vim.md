---
title: vim
date: 2021-03-23 07:21:23
categories:
- 工具
tags:
- vim
---


**NOTE：**

vim作为文本编辑器，虽然其本质也一直在变在发展。但是，最根本的作为编辑器来说。不应该去追求花哨看似有用的功能。而应该在这些实用方面深入：
- 快速移动，和定位。比鼠标移动的快才算合格
- 快速搜索文件，创建文件，删除文件，创建文件夹，删除文件夹
- 快速字符串查找



### windows平台构建编译

[msvc编译步骤官方文档](https://github.com/vim/vim/blob/master/src/INSTALLpc.txt)

```
1. 进入src目录

2. nmake -f Make_mvc.mak PYTHON3=C:\Users\ming\AppData\Local\Programs\Python\Python310 DYNAMIC_PYTHON3=yes PYTHON3_VER=310

3. 在顶级目录下查看是否有vim90目录，没有创建

4. Copy the "runtime" files into "vim90":
copy runtime\* vim90
xcopy /s runtime\* vim90

5. Copy the new binaries into the "vim90" directory
copy src\*.exe vim90
copy src\tee\tee.exe vim90
copy src\xxd\xxd.exe vim90

6. Copy gettext and iconv DLLs into the "vim90" directory【从E:/vim/vim90中搜索dll，拷贝到新的vim90中】

7. 执行老vim90中的uninstall.bat,删除老的vim90
8. 将新vim90拷贝到E:/vim中，并执行vim90中的install.bat

```





## vim脚本语法

### 变量类型：

The following builtin types are supported:
        bool
       number
       float
       string
       blob
       list<{type}>
       dict<{type}>
       job
       channel
       func
       func: {type}
       func({type}, ...)
       func({type}, ...): {type}
       void

### 变量作用域

-   全局变量	g:name
-   脚本范围局部变量    s:name 
-   vim预定义变量    v:name 
-   函数内局部变量：l:name 
-   函数参数：a:name 
-   缓冲区局部变量：b:name 
-   窗口局部变量：w:name 
-   标签页局部变量：t:name 

>   脚本范围的局部变量，相当于c语言中static关键字，定义之后，只在当前声明的文件范围有效。

<br>

### 变量定义：

```shell
let g:a = 123
let s:b = "adf"
```



### 取消变量定义：

```shell
unlet s:a
```

### 表达式
1 基本表达式
    数值
    字符串常量
    $name 环境变量
    &name 选项
    @reg  寄存器
2 算法表达式
    + - * / %
    "hello" . "world" =="helloworld" 字符串拼接
### 函数
call function_name(para1,para2) 	调用函数
let line = getline('.') 			调用函数作为表达式
:functions 							查看vim中内置的完整函数列表

1 定义函数
    function {name}({var1,{var2},...})
        {body}
    endfunction
    - 函数名必须以大写字母开始
    - a:var1 代表函数参数变量
    - 函数内部没有特殊标识的变量(g: a: s:)都是局部变量
    - 函数内部访问全局变量要加 g:
2 重定义函数
    function! name(var1,var2)
3 范围使用
    :10,30call funcname()
        function funcname() range 	定义
4 可变参数
    function Show(start,...)
    a:0 	可变参数个数
    a:1 	第一个可变参数
    a:2 	第二个可变参数
    a:000 	可变参数列表
5 删除函数
    delfunction xxx
