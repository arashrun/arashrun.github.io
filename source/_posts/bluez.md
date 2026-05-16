---
title: bluez
date: 2026-05-06 16:23:57
categories:
  - linux
tags:
  - 蓝牙
---

在 Linux 下，蓝牙功能通常由 **BlueZ** 协议栈提供，其管理和使用主要通过命令行工具完成。以下是常用命令及场景。

---

## 一、核心管理工具：`bluetoothctl`（推荐）

`bluetoothctl` 是 BlueZ 提供的交互式命令行工具，也是目前最通用的蓝牙管理方式。

### 1. 进入交互式 shell
```bash
bluetoothctl
```
进入后提示符变为 `[bluetooth]#`，可执行以下子命令。

### 2. 常用子命令

| 功能 | 命令 |
|------|------|
| 查看控制器状态 | `show` |
| 打开蓝牙电源 | `power on` |
| 关闭蓝牙电源 | `power off` |
| 开始扫描设备 | `scan on` |
| 停止扫描 | `scan off` |
| 列出已发现设备 | `devices` |
| 配对设备 | `pair <MAC>` |
| 移除配对设备 | `remove <MAC>` |
| 连接设备 | `connect <MAC>` |
| 断开连接 | `disconnect <MAC>` |
| 信任设备（自动连接） | `trust <MAC>` |
| 查看已连接设备 | `info <MAC>` |
| 退出 | `exit` 或 `quit` |

### 3. 非交互式使用（直接执行单条命令）
```bash
bluetoothctl -- power on
bluetoothctl -- scan on
bluetoothctl -- devices
bluetoothctl -- pair AA:BB:CC:DD:EE:FF
```

---

## 二、传统命令工具（部分发行版已废弃）

### 1. `hciconfig` – 配置蓝牙控制器
```bash
hciconfig                 # 查看所有控制器
hciconfig hci0 up         # 启用 hci0
hciconfig hci0 down       # 禁用
hciconfig hci0 name "myBT"  # 设置名称
```

### 2. `hcitool` – 扫描、查询等
```bash
hcitool dev               # 列出本地控制器
hcitool scan              # 扫描传统蓝牙设备
hcitool lescan            # 扫描低功耗蓝牙设备
hcitool info AA:BB:CC:DD:EE:FF   # 获取设备信息
```

> 注意：`hciconfig` 和 `hcitool` 在较新发行版中可能已移除，推荐使用 `bluetoothctl`。

---

## 三、蓝牙服务管理

### 1. 启动/停止蓝牙服务
```bash
sudo systemctl start bluetooth
sudo systemctl stop bluetooth
sudo systemctl enable bluetooth   # 开机自启
```

### 2. 查看蓝牙服务状态
```bash
systemctl status bluetooth
```

### 3. 射频控制（软件开关）
```bash
rfkill list                 # 查看所有无线设备（含蓝牙）
rfkill block bluetooth      # 软关闭蓝牙
rfkill unblock bluetooth    # 软开启蓝牙
```

---

## 四、文件传输（OBEX）

需要安装 `obexftp` 或使用 `bluetoothctl` 配合。

### 使用 `bluetoothctl` 发送文件（需要配对）
```bash
bluetoothctl
# 配对并连接后
send <MAC> /path/to/file
```

### 使用 `obexftp`（传统方式）
```bash
# 发送文件
obexftp -b AA:BB:CC:DD:EE:FF -p /path/to/file
# 接收文件（等待）
obexftp -b AA:BB:CC:DD:EE:FF -g
```

---

## 五、查看蓝牙日志与调试

```bash
# 实时查看蓝牙系统日志
sudo journalctl -u bluetooth -f

# 查看蓝牙适配器详细信息
cat /sys/class/bluetooth/hci0/address   # 查看MAC地址
hciconfig -a                            # 详细硬件信息
```

---

## 六、常见问题处理

| 问题 | 解决方案 |
|------|----------|
| `bluetoothctl` 找不到设备 | 检查 `rfkill list` 是否被 block；启动服务 `sudo systemctl start bluetooth` |
| 配对失败 | 先 `remove <MAC>` 再重新配对；确认双方都进入配对模式 |
| 连接后自动断开 | 使用 `trust <MAC>` 设置信任 |
| 无法扫描到蓝牙耳机 | 耳机需处于配对模式；尝试 `scan on` 后等待 |

---

## 七、图形化替代（非命令）

- **`blueman`**：图形化的蓝牙管理器，提供系统托盘图标。
- **`gnome-bluetooth`**：GNOME 桌面集成。

---

## 总结：最常用命令速查

```bash
bluetoothctl power on
bluetoothctl scan on
bluetoothctl devices
bluetoothctl pair AA:BB:CC:DD:EE:FF
bluetoothctl connect AA:BB:CC:DD:EE:FF
bluetoothctl trust AA:BB:CC:DD:EE:FF
bluetoothctl disconnect AA:BB:CC:DD:EE:FF
```

使用 **`bluetoothctl`** 基本可以完成所有蓝牙管理任务。