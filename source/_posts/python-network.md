---
title: python_network
date: 2023-04-01 10:13:37
categories:
- python
tags:
- 网络
---


### socket


### asyncore

- 为了后向兼容，新代码建议使用 `asyncio` 。
- 模块提供了基本的基础设施，用于实现异步socket服务器和socket客户端
- 单处理器实现同时做多件不同的事只有两种方式，一种是多线程，另一种是使用现代操作系统提供的io多路复用系统调用：select，poll。
- 该模块基本的思路是：通过继承 `asyncore.dispatcher` 或 `asyncore.async_chat` 并实现多个channel实例，然后将这些channel添加到全局的一个map中进行维护，一般是在loop函数中提供了该全局map。这个loop会等待所有handle处理完成才会退出关闭
- `asyncore.dispatcher` 作为channel的实现，使用者继承该类之后，通过override重写父类中的如下方法，可以实现自定义读写等操作
	- handle_read
	- handle_write
	- handle_connect
	- handle_close
	- handle_accept
- 此外 `asyncore.dispatcher` 作为socket对象的代理，也提供了一些与原生socket一致的接口，他们的参数很多与原生socket是一致的
	- create_socket
	- connect
	- send
	- recv
	- listen
	- bind
	- accept
	- close

### asyncio

