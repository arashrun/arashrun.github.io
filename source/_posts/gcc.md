---
title: gcc
date: 2025-05-05 01:04:32
categories:
  - 工具
tags:
---

## 常用命令

如下命令常用与排查交叉编译相关问题

```bash
# readelf命令用于查看ELF文件中的符号表和版本信息，属于binutils包的一部分
readelf -sV /lib/aarch64-linux-gnu/libc.so.6 |grep mesa
    36: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND mesa_memcpy@GLIBC_2.17 (2)
    41: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND mesa_memmove@GLIBC_2.17 (2)
	51: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND mesa_memset@GLIBC_2.17 (2)
	
# 查看gcc编译器的库文件搜索路径
/usr/bin/aarch64-linux-gnu-g++-8 --sysroot=/mnt/kylinOS -print-search-dirs

# 使用 strings 查看libc库最高支持的版本
strings $(/usr/bin/aarch64-linux-gnu-g++-8 -print-file-name=libc.so.6) | grep GLIBC_

# 我们可以通过gcc的 `-print-file-name=libc.so.6` 的选项来查看编译器内部自带的动态库路径
# 找到编译器对应的 libc.so 路径
/usr/bin/aarch64-linux-gnu-g++-8 -print-file-name=libc.so.6

```




**问题引入：**

```c
void test(int) {
}

int main() {
    return 0;
}

```

上面的这代代码在Ubuntu20中使用默认gcc版本是无法编译通过的。会报如下错误：

> main.c: In function ‘test’:
main.c:2:11: error: parameter name omitted
    2 | void test(int)
      |           ^~~

在 `c2x` 标准之前的c标准中，是不允许忽略掉参数名称的。这一点和c++ 不同。这个特性是在 `c2x` 中才引入的。

- Ubuntu20中，默认安装的gcc版本是gcc-9.x版本的，并且最高的版本是gcc-10.5
- 需要安装11版本或更高的gcc版本。[ubuntu20.04中升级gcc,g++到gcc-11,g++11 - Linux开发笔记](https://cfanzp.com/cpp-ubuntu2004-gcc11/)


```shell
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 11 --slave /usr/bin/g++ g++ /usr/bin/g++-11

 sudo update-alternatives --config gcc
```

