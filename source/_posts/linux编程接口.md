---
title: linux编程
date: 2022-11-23 09:17:58
categories:
- linux
tags:
- linux
---





### syscall table

| syscall              | 类别   | 含义 |
|----------------------|--------|------|
| io_setup             | 异步io |      |
| io_destroy           |        |      |
| io_submit            |        |      |
| io_cancel            |        |      |
| io_getevents         |        |      |
| io_getevents_time32  |        |      |
| io_pgetevents        |        |      |
| io_pgetevents_time32 |        |      |
| io_uring_setup       |        |      |
| io_uring_enter       |        |      |
| io_uring_register    |        |      |
| setxattr             |        |      |
| lsetxattr            |        |      |
| fsetxattr            |        |      |
| getxattr             |        |      |
| lgetxattr            |        |      |
| fgetxattr            |        |      |
| listxattr            |        |      |
| llistxattr           |        |      |
| flistxattr           |        |      |
| removexattr          |        |      |
| lremovexattr         |        |      |
| fremovexattr         |        |      |
| getcwd               |        |      |
| lookup_dcookie       |        |      |
| eventfd2             |        |      |
| epoll_createl        |        |      |
| epoll_ctl            |        |      |
