---
title: hostapd
date: 2026-01-20 15:29:29
categories:
  - linux
tags:
  - network
---
## AP热点获取连接的设备 

在使用 hostapd 开启热点后，查看连接设备的方法主要有两种思路：一是直接询问 hostapd 的控制端（最准确），二是通过网络层工具查看（更详细）。

### 方法 1：使用 hostapd_cli (最直接)

hostapd_cli 是 hostapd 自带的命令行控制工具，可以直接获取实时连接的无线客户端 MAC 地址。

**1. 查看所有已连接设备的 MAC 地址：** 

```bash
sudo hostapd_cli all_sta
```

这条命令会输出一大串信息。如果你只想看连接了几个设备以及它们的 MAC 地址，可以过滤一下：

```bash
sudo hostapd_cli all_sta | grep dot11RSNAStatsSTAAddress
```

**2. 交互模式：** 

你也可以直接输入 sudo hostapd_cli 进入交互界面，然后输入 all_sta 查看。

---

### 方法 2：使用 iw 工具 (底层查看)

如果你想看网卡物理层面上关联了哪些设备，可以使用 iw 命令：

```bash
# 假设你的无线网卡接口名是 wlan0
sudo iw dev wlan0 station dump
```

输出内容包括：
● Station [MAC 地址]
● 信号强度 (signal)
● 数据传输速率 (tx/rx bitrate)
● 连入时长 (connected time)

---

### 方法 3：查看 IP 和主机名 (最实用)

hostapd 只负责无线链路层（L2），不负责分配 IP。IP 通常是由 dnsmasq 或 isc-dhcp-server 分配的。要查看连接设备的 IP 和名字，查看 DHCP 租约记录最方便。

如果你使用的是 dnsmasq（大多数教程的推荐做法）：

```bash
cat /var/lib/misc/dnsmasq.leases
```

输出格式：
> [过期时间] [MAC 地址] [IP 地址] [主机名] [客户端 ID]

如果你使用的是 isc-dhcp-server：
Bash

```bash
cat /var/lib/dhcp/dhcpd.leases
```

---

### 方法 4：使用 arp 或 ip neigh (通用方法)

如果设备已经连上并有了网络活动，可以通过 ARP 缓存表查看：

```bash
# 查看 ARP 表
arp -a

# 或者使用更现代的 ip 命令
ip neighbor show dev wlan0
```

---

总结建议
● 只想看有多少人连着： 用 `sudo hostapd_cli all_sta` 。
● 想知道连进来的是谁（手机还是电脑）： 查看 `/var/lib/misc/dnsmasq.leases`  获取主机名和 IP。
● 想看信号好不好： 用 `sudo iw dev wlan0 station dump` 。
如果你发现有陌生设备连入，需要我帮你写一段脚本来实现“MAC 地址白名单”过滤吗？