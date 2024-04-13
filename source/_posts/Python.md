---
title: python术语
date: 2021-06-18 12:23:19
categories:
- python
tags:
- 学习术语
---

- module(模块)
python代码组织单元
- package（包）
一种特殊的module，包含__path__属性。在一个包中可以包含其他子模块，和子包。有常规包和命名空间包之分，常规包就是包含__init__.py文件的目录，命名空间包只包含其他子包的，命名空间包没有物理实体对应，这一点不同于常规包。
- 表达式
- 语句
- 推导式
- 生成器（generator）
生成器是一个函数，可以返回生成器迭代器对象的函数。和普通函数的区别就是，普通函数的return语句替换为yield表达式。
- 生成器表达式
使用"()"包裹起来的表达式，与推导式的区别在于使用的是()包裹，而不是推导式的[]或者{},生成器表达式会产生生成器对象。
- 生成器函数，异步生成器函数
- yield表达式
yield 表达式在定义[[|生成器函数]]或[[|异步生成器函数]]时才会用到因此只能在函数定义的内部使用


## 标准库

- [x] yield关键字
- [ ] pdb:python命令行debug工具


1. yield关键字
[Python 2.2 有什么新变化 — Python 3.10.7 文档](https://docs.python.org/zh-cn/3/whatsnew/2.2.html?#pep-255-simple-generators)
根据文档解释，yield是2.2版本引入的新关键字。yield是一种用来控制函数运行的方式，一般函数通过return返回，而yield出现的地方是返回一个generator（生成器）对象。当外部调用生成器的`next()`方法后，函数会继续运行yield之后的语句。

```python
def generate_ints(N):
    for i in range(N):
        yield i+1
```
调用上面的函数，函数会运行到yield这里会直接挂起改函数，并返回一个generator对象。外部可以通过next方法，再次恢复(resume)函数的执行。并从yield之后开始执行，也就是执行`i+1`

