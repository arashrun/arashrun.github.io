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

**注意：**
这种方式当`run` 函数执行完成线程就退出了。并且这种方式除非你手动调用`exec()` 否则是没有任何事件循环运行在线程中的。

记住，QThread对象本身是存在于创建它的（老）线程中的，而不是在`run` 方法所在的新线程中。这意味着所有以队列方式调用的slots以及invoked的方法都会在老线程中执行，因此开发人员如果想让槽函数在新线程中执行必须使用下面的方式二(movetothread)。

QThread的构造函数是在老线程中执行的，而run方法是在新线程中执行。如果成员变量既在继承的Qthread的构造函数中使用，又在run方法中被使用，呢么这个成员变量就是被两个线程所使用，需要考虑线程安全问题了。

2. moveToThread方式(属于QObject类的一个方法)
	1. 任务类继承QObject类
	2. 添加任务方法
	3. 主线程中创建QThread类，和任务类
	4. workclass->moveToThread(thread_1)


**线程管理**

信号通知：
```bash
线程开始——started
线程结束——finished
```

管理方法：
```bash
查询状态——isFinished/isRunning
停止线程——exit/quit/terminate
休眠线程——sleep/msleep/usleep
```


### 线程池——QThreadPool


使用方式：

1. 任务类继承QRunnable类，重写纯虚函数run
1. 调用QThreadPool静态方法 `QThreadPool::globalInstance()` 返回全局线程池对象
2. 调用全局线程池对象的start，将任务类实例添加进去


