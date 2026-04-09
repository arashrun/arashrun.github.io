---
title: thunderbird替代outlook
date: 2026-04-09 09:56:11
categories:
  - 工具
  - 小技巧
tags:
  - 日常使用
---


### 常见协议

**接收邮件的协议:**

imap: 邮件存储在邮件服务器端，对邮件的所有操作都会同步到服务器，适合多端同步，是目前主流的接收协议
pop3: 邮件在一个客户端下载下来之后，就会从服务器删除掉。其他客户端无法查看了。适用于老式，特殊目的使用的协议

**发送邮件协议：**

smtp：simple mail transfer protocal(简单邮件传输协议)


**Exchange Active Sync(EAS):**

由微软开发，Exchange协议可供用户同步邮件、联系人、日历及其他所有Exchange对象。有相关的python库和c++库可以使用


### 日程同步方案

同步日程可以使用Exchange协议提供的日历同步功能，目前QQ邮箱支持Exchange协议。需要在安全设置中开启 `imap/pop3/exchange/caldav` 选项，之后生成一个密钥。
其次需要一个支持exchange账号登录的客户端，这里吐槽一下outlook，微软为了推自家的office 365套件，强行将exchange绑定到365账号，新版本outlook拉完了。这里推荐使用thunderbird这个开源邮箱客户端，支持跨平台。需要安装 `TbSyn` 和 `EAS` 插件即可配置连接qq的exchange服务器 `ex.qq.com` 。exchange默认使用443端口。
配置完成之后，就可以在windows上通过thunderbird客户端增删改日程信息了。之后手机端，打开日历应用，找到日程导入，配置从exchange支持的qq邮箱中导入即可同步到手机端

