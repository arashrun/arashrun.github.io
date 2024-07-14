---
title: 基于autoxjs的实时公交提醒
date: 2024-04-14 17:18:08
categories:
  - 工具
tags:
  - autoxjs
  - android
  - 自动化脚本
---


### 实现流程

1. 基于高德地图的实时公交功能
2. 通过autoxjs的无障碍服务的功能，实现自动化解锁，打开软件，找到实时公交界面，获取公交到站剩余时间，并通过Android系统的语音助手功能，进行实时语音播报
3. 使用Android系统提供的定时启动软件的功能，实现对我们的autoxjs打包之后的应用的一个定时启动，从而完成工作日的公交叫醒服务


### 可以深入的方向

- Android系统的定时服务如何实现
- Android系统的tts服务框架实现
- autoxjs基于的 `Rhino` 引擎和Java原生互调的方式