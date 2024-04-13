---
title: c++并发编程
date: 2022-04-07 22:31:08
categories:
- c++
tags:
- c++
- 并发编程
---



### thread

<u style="color:red">Atomic operations</u>

> These components are provided for fine-grained atomic operations allowing for lockless concurrent programming. Each atomic operation is indivisible with regards to any other atomic operation that involves the same object. Atomic objects are free of data races.
> 这些组件提供了良好的粒度，原子的操作允许不通过锁的方式就可以做到同步编程。每一个原子操作都是不可分割的，对于其他的原子操作对象并且作用在同一个对象上，原子对象对于数据竞争是自由的free of data races

<u style="color:red">Mutual exclusion</u>

> Mutual exclusion algorithms prevent multiple threads from simultaneously accessing shared resources. This prevents data races and provides support for synchronization between threads.<br><br>
> 互斥量算法阻止多个线程同时访问共享资源，它可以防止数据竞争，提供了多个线程之间同步的机制。

Mutual exclusion (互相的+排斥的 = 互斥量)
simultaneously (同时的)
synchronization (同步性)

通常互斥量mutex不会单独使用，就像new和delte一样容易忘记释放。建议使用自动管理类（lock_guard,unique_lock)等来间接使用。

- lock_guard(是一个mutex的简单封装，构造时加锁，析构时自动解锁）
- unique_lock：(提供了更多的功能相比于lock_guard，配合条件变量使用）

<u style="color:red">Condition variables</u>

> A condition variable is a synchronization primitive that allows multiple threads to communicate with each other. It allows some number of threads to wait (possibly with a timeout) for notification from another thread that they may proceed. A condition variable is always associated with a mutex.<br><br>
> 一个条件变量是一个同步原语,同步的意思是允许多个线程互相交流。它允许一些数量的线程去等待另外的线程发起通知唤醒。一个条件变量通常和一个互斥量（mutex）联系在一起



<u style="color:red">Semaphores</u>

> A semaphore is a lightweight synchronization primitive used to constrain concurrent access to a shared resource. When either would suffice, a semaphore can be more efficient than a condition variable.<br><br>
> 一个**信号量**是一个轻量级的同步原语，用于限制同时访问共享资源。当都没有suffice的时候，使用信号量比条件变量更加有效。


<u style="color:red">Latches and Barriers</u>

(since c++20)

> Latches and barriers are thread coordination mechanisms that allow any number of threads to block until an expected number of threads arrive. A latch cannot be reused, while a barrier can be used repeatedly.<br><br>
> 门闩和栅栏都是线程协作机制，它们允许任意数量的线程阻塞直到一些被期望的线程到达。门闩不能够被重复使用，而栅栏可以被重复使用多次。

<u style="color:red">Futures</u>

> The standard library provides facilities to obtain values that are returned and to catch exceptions that are thrown by asynchronous tasks (i.e. functions launched in separate threads). These values are communicated in a shared state, in which the asynchronous task may write its return value or store an exception, and which may be examined, waited for, and otherwise manipulated by other threads that hold instances of std::future or std::shared_future that reference that shared state.<br><br>
> 标准库提供的一个机制，用来获取那些异步运行的任务的返回值和异常。这些返回值和异常通过共享的状态来交流，异步任务会将它的返回值写入或者存储它的异常到这个共享状态中。之后那些拥有std::future or std::shared_future对象的线程就可以通过这些对象来检查，等待或者操作这些返回值和异常。（这里的共享状态就是std::future or std::shared_future)


