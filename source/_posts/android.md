---
title: android
date: 2022-10-16 10:41:09
categories:
  - android
tags:
  - android
---

### 基本概念

#### 第一章

- 【activity】：表示应用中的一个界面
- 【service】：是一个不使用用户界面而在后台执行操作的组件
- 【broadcast】：是一种消息类型，任何应用都可以接受
- 【intent】：是消息传递的载体
- 【显式 Intent】：提供了消息传递的目标地址的intent
- 【隐式intent】：没有提供明确目标地址的intent，而是提供了想要执行的操作，让系统中其他匹配的应用提供服务。[隐式intent处理逻辑](#隐式intent处理逻辑)
- 【intent过滤器】：是[应用清单文件](#应用清单文件)中的一个表达式，用于指定该应用可以接收的intent类型







### 展开

##### 应用清单文件

[清单文件](https://developer.android.google.cn/guide/topics/manifest/manifest-intro)



##### 隐式intent处理逻辑

使用隐式 Intent 时，Android 系统通过将 Intent 的内容与在设备上其他应用的清单文件中声明的 Intent 过滤器进行比较，从而找到要启动的相应组件。如果 Intent 与 Intent 过滤器匹配，则系统将启动该组件，并向其传递对象。如果多个 Intent 过滤器兼容，则系统会显示一个对话框，支持用户选取要使用的应用。



#### NDK

> Android NDK 是一组使您能将 C 或 C++（“原生代码”）嵌入到 Android 应用中的工具

