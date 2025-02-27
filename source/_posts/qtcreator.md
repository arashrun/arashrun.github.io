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


# 修改程序的ld 【出现段错误的时候】
sudo patchelf --set-interpreter /opt/qt-creator/libc/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2  qtcreator


# 二级依赖导致先加载的系统本地的libpthread版本，虽然上面指定了rpath，但是need查找优先级高于rpath
sudo patchelf --add-needed libpthread.so.1 qtcreator
```

同理，其他小工具无法使用的时候也按照上面的指令来修改elf头即可


## qmake构建系统


**交叉编译**

1. 安装交叉编译器
2. 创建目标平台的根文件系统img，并进行挂载
3. 创建qmake的配置文件

```qmake

#!/bin/bash

script_dir_path=`dirname $0`
script_dir_path=`(cd "$script_dir_path"; /bin/pwd)`

#/usr/lib/qt5/bin/qmake -qtconf "$script_dir_path/arm.conf" $*
#/usr/bin/qmake -qtconf "$script_dir_path/arm.conf" $*

#/usr/lib/x86_64-linux-gnu/qt5/bin/qmake $* -qtconf "$script_dir_path/arm.conf"
/usr/bin/qmake $* -qtconf "$script_dir_path/arm.conf"

```


```arm.conf

[Paths]
Prefix=/usr
ArchData=lib/aarch64-linux-gnu/qt5
Binaries=../../../usr/lib/qt5/bin
Data=share/qt5
Documentation=share/qt5/doc
Examples=lib/aarch64-linux-gnu/qt5/examples
Headers=include/aarch64-linux-gnu/qt5
Imports=lib/aarch64-linux-gnu/qt5/imports
Libraries=lib/aarch64-linux-gnu
LibraryExecutables=lib/aarch64-linux-gnu/qt5/libexec
Plugins=lib/aarch64-linux-gnu/qt5/plugins
Qml2Imports=lib/aarch64-linux-gnu/qt5/qml
Settings=/etc/xdg
Translations=share/qt5/translations
Sysroot=/mnt/kylinOS
SysrootifyPrefix=true
TargetSpec=linux-aarch64-gnu-g++
HostSpec=linux-g++
HostPrefix=/usr
;HostPrefix=/mnt/kylinOS
;HostBinaries=lib/qt5/bin
;HostData=lib/x86_64-linux-gnu/qt5
HostData=../mnt/kylinOS/usr/lib/aarch64-linux-gnu/qt5
;HostLibraries=lib/x86_64-linux-gnu
;HostLibraryExecutables=libexec
```



对于libgl动态库的问题，需要修改img镜像中的qt的mkspec文件。覆盖 `QMAKE_LIBS_OPENGL` 


```qmake.conf

load(qt_config)  # 加载 Qt 默认配置

# 覆盖 OpenGL 配置（必须在此位置）
QMAKE_LIBS_OPENGL = $$[QMAKE_SYSROOT]/usr/lib/aarch64-linux-gnu/libGL.so
```


