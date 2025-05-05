---
title: gcc
date: 2025-05-05 01:04:32
categories:
  - 工具
tags:
---

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

