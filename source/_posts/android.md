---
title: android
date: 2022-10-16 10:41:09
categories:
- android
tags:
- android
---

### 概念表

1. 【activity】：表示应用中的一个界面
2. 【service】：是一个不使用用户界面而在后台执行操作的组件
3. 【broadcast】：是一种消息类型，任何应用都可以接受
4. 【intent】：是消息传递的载体
5. 【显式 Intent】：提供了消息传递的目标地址的intent
6. 【隐式intent】：没有提供明确目标地址的intent，而是提供了想要执行的操作，让系统中其他匹配的应用提供服务。[[#^0fb03d|细节]]
7. 【intent过滤器】：是[[#^2df5a3|应用清单文件]]中的一个表达式，用于指定该应用可以接收的intent类型







### 展开

**应用清单文件** ^2df5a3
[清单文件](https://developer.android.google.cn/guide/topics/manifest/manifest-intro)



**隐式intent处理逻辑** ^0fb03d

使用隐式 Intent 时，Android 系统通过将 Intent 的内容与在设备上其他应用的清单文件中声明的 _Intent 过滤器进行比较，从而找到要启动的相应组件。如果 Intent 与 Intent 过滤器匹配，则系统将启动该组件，并向其传递对象。如果多个 Intent 过滤器兼容，则系统会显示一个对话框，支持用户选取要使用的应用。