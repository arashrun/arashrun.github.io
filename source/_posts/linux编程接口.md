---
title: linux编程
date: 2022-11-23 09:17:58
categories:
- linux
tags:
- linux
---
[系统调用reference文档](https://oikvmsuwqias.ap-southeast-1.clawcloudrun.com/d/cm/Linux_Syscall_quickref.pdf?sign=iAwC56xhptiwl1AOpqa5KgJJtGCG2gFIBx0TYXt0HFo=:0) 




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
