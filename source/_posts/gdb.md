---
title: gdb
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
- gdb
- 调试
---


# gdb 调试

## 常用命令

gcc -g
g++ -ggdb3      # 生成调试版本

file <exe>      # 加载可执行文件

run [argv]      # 一直运行,直到碰到breakpoint，或带命令行参数argv运行
start           # 执行到main下第一行停止


step/s          # 进入当前行, 逐步运行
next/n          # 运行到下一行
countinue/c     # 继续run运行，到下一个break点或错误、正常退出

list            # 显示main所在文件源码
list file.c:10  # 显示指定文件某行

break/b 10      # 指定行打断点
b file.c:10     #

bt              # 打印当前栈帧 == where
where           # 同上
up              # 跳到上一个栈
down            # 跳到下一个栈


watch exp       # 监视表达式，常用于循环变量，运行过程中变化

## tips
revert-step     # 回退执行


## layout 图像交互界面

ctrl-x a        # 进入/退出 交换界面
ctrl-x 1        #
ctrl-x 2        #

ctrl-l          # 刷新界面输出

```
# ~/.gdbinit      # 用于自动刷新tux界面

define c 
  continue
  refresh
end

define n
  next
  refresh
end

```

layout
    - src       # 显示源码窗口
    - asm       # 显示汇编窗口
    - reg       # 显示源码、汇编、寄存器窗口
    - split     # 源码、汇编窗口
    - next      # 类似tabnext，下一个窗口
    - prev      # 与next相对


info source     # 显示编译时的源码目录信息
show dir        # 显示gdb的源码搜索路径
dir             # 添加gdb的默认源码搜索路径
[解决gdb调试找不到源码的问题](https://blog.csdn.net/albertsh/article/details/107437084)
