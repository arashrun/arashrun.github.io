---
title: qt重点内容
date: 2021-07-28 09:46:50
tags:
---


## qt项目的文件类型

1. \*.pro(项目描述文件)
```
qmake -project "QT+=widgets"
```
2. \*.qrc(项目的临时资源文件)
3. \*.rcc(项目代码中使用的资源文件)
```
# qt中使用资源有两种方式：
1. 作为外挂资源使用(将*.qrc转换为*.rcc文件，在代码中进行注册使用)
rcc -binary myresource.qrc -o myresource.rcc
QResource::registerResource("/path/to/myresource.rcc");
2. 作为内嵌资源使用(*.qrc转换为qrc_*.cpp加入代码编译)
rcc -name application  application.qrc -o qrc_application.cpp
```
4. \*.ui(界面的描述文件，xml格式的)
```
uic hello.ui -o ui_hello.h
```

5. 包含Q_OBJECT的头文件(该宏用在含有槽函数的文件中)
```
moc hellowidget.h -o moc_hellowidget.cpp (hellowidget中的类中含有该宏)
```

## qt项目书写流程
1. 创建ui文件
2. 使用uic命令生成ui_\*.h头文件
3. 书写主程序
4. moc生成元对象代码moc_xxx.cpp

## qt构建过程
1. qmake -project "QT+=widgets"
qmake根据项目中的文件，自动生成pro项目配置文件
2. qmake
第二次执行qmake生成makefile
3. make

## 核心概念的解释

### qt元对象系统
元对象系统室qt专门对c++做的扩展，用来支持信号和槽机制、运行时类型定义、动态属性系统。


## Reference:
> 1. [中文文档](https://qtguide.ustclug.org/)
> 2. [官方文档](https://doc.qt.io/qt-5/topics-network-connectivity.html)


## TODOLIST
1. qwidgetstack用于多个widget组合切换
2. model/view framework
3. 代码中修改动态属性值
    widget->setProperty("property", value);
4. animation framework

