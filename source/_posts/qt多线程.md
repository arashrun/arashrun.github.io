---
title: qt多线程
date: 2023-05-23 10:29:48
categories:
- qt
tags:
- 多线程
---

[08-线程的使用方式2-添加修改任务类_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1iN411f7dY?p=8&vd_source=0a2bd2d5e3c437b3fd7699cd52ebe78d)


### 线程类——QThread

两种使用方式：

1. 继承QThread类
	1. 任务类继承QThread类
	2. 重写QThread的run方法，实现子线程业务逻辑
2. moveToThread方式(属于QObject类的一个方法)
	1. 任务类继承QObject类
	2. 添加任务方法
	3. 主线程中创建QThread类，和任务类
	4. workclass->moveToThread(thread_1)


### 线程池——QThreadPool


使用方式：

1. 任务类继承QRunnable类，重写纯虚函数run
1. 调用QThreadPool静态方法 `QThreadPool::globalInstance()` 返回全局线程池对象
2. 调用全局线程池对象的start，将任务类实例添加进去


