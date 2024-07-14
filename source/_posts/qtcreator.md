---
title: 低版本运行高版本程序的方法
date: 2024-07-13 23:55:19
categories:
  - 工具
tags:
  - elf
  - patchelf
---


### 安装

在低版本的平台运行高版本的qtcreator的方法！！！


1. github上下载最新qtcreator
2. dpkg -i xxx.deb
3. 手动下载libc.deb和libstdc++6.deb
4. github下载patchelf工具
5. 修改qtcreator的elf
```bash
# 添加libc和libstdc++库的链接路径到elf的rpath中
sudo patchelf --add-rpath /opt/qt-creator/libc/lib/x86_64-linux-gnu/:/opt/qt-creator/libstd/usr/lib/x86_64-linux-gnu/ qtcreator


# 修改程序的ld
sudo patchelf --set-interpreter /opt/qt-creator/libc/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2  qtcreator
```