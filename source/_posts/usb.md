---
title: usb
date: 2026-01-21 08:59:17
categories:
tags:
---

## usb设备模型


## gadget、dwc3、udc、rndis


gadget = Linux变身USB设备的框架
dwc3 = 能切换主/从模式的智能usb控制器
udc = usb device control = usb设备控制器
rndis = 远程网络驱动接口规范 = usb设备上传输tcp/ip的一种规范


```
┌─────────────────────────────────────────────────────────┐
│                Linux USB Gadget架构                      │
├─────────────────────────────────────────────────────────┤
│ 用户空间应用                                             │
│       ↓                                                 │
│ 功能驱动 (Function Drivers)                             │
│  ┌─────┴─────┐                                          │
│  f_rndis.ko  f_mass_storage.ko  f_serial.ko             │
│  (网络)      (U盘)              (串口)                  │
│       ↓                                                 │
│ 复合设备框架 (Composite Framework)                      │
│  └─────┬─────┘                                          │
│  libcomposite.ko                                        │
│       ↓                                                 │
│  UDC核心 (UDC Core)                                     │
│  └─────┬─────┘                                          │
│  USB设备控制器驱动 (如dwc3.ko)                           │
│       ↓                                                 │
│ 硬件: USB PHY + 连接器                                   │
└─────────────────────────────────────────────────────────┘
```


## udc和dwc3

简单来说，**UDC 是一个“职业岗位”（通用接口），而 DWC3 是在这个岗位上工作的“具体员工”（硬件驱动）。**

在 Linux USB 子系统中，它们的关系可以从以下几个维度来理解：

### 1. 核心定义

* **UDC (USB Device Controller)**：即 **USB 设备控制器**。
* 它是一个**通用的软件框架/子系统**。
* 它的作用是定义一套标准接口，让上层的 USB 功能（如模拟鼠标、U盘、网卡等 Gadget 驱动）不需要关心底层硬件的具体细节。
* 在 Linux 路径通常对应 `drivers/usb/gadget/udc/`。


* **DWC3 (DesignWare Core USB3)**：由 Synopsys 公司设计的 **USB 3.0 控制器 IP 核**。
* 它是一个**具体的硬件控制器**及其对应的**底层驱动程序**。
* DWC3 非常强大，通常支持双角色（Dual-Role），既能当主机（Host），也能当设备（Device）。
* 在 Linux 路径通常对应 `drivers/usb/dwc3/`。



### 2. 它们之间的关系

**DWC3 是 UDC 框架下的一种具体实现。**

如果你把 Linux USB 体系看作一家公司：

* **USB Gadget 驱动**（如 Mass Storage, ADB）是“业务部”，负责实现具体功能。
* **UDC 框架**是“管理层”，定义了业务部如何调用底层硬件的规章制度。
* **DWC3 驱动**是“技术部具体员工”，它负责直接操作硬件寄存器，并向 UDC 框架注册自己。

当 DWC3 工作在 **Device 模式**（从机模式）时，它会向 UDC 子系统注册，告诉内核：“我是一个可用的 USB 设备控制器”。

### 3. 层级结构图

在 Linux 内核中，数据流和控制流的层级如下：

1. **用户层 / Gadget 功能层** (ConfigFS / FunctionFS)
* *定义你想让设备变成什么（例如：U盘、串口）。*


2. **USB Gadget 复合层** (libcomposite)
* *处理枚举、描述符分配等通用 USB 逻辑。*


3. **UDC 核心框架** (UDC Core)
* *承上启下，管理所有可用的控制器。*


4. **DWC3 驱动层** (Hardware Specific Driver)
* *控制 DWC3 硬件寄存器，处理 DMA、端点（Endpoint）中断。*


5. **DWC3 硬件** (Physical IP Core)
* *物理上的电路芯片。*



### 4. 常见操作中的联系

当你使用 `configfs` 配置 USB Gadget 时，最后一步通常是：

```bash
echo "fe800000.dwc3" > UDC

```

这行命令的意思就是：**告诉 UDC 框架，请把当前配置好的 USB 功能绑定到名为 `fe800000.dwc3` 的硬件控制器上运行。**

### 总结

* **UDC** 是抽象的**接口层**，负责管理。
* **DWC3** 是具体的**驱动层**，负责干活。
* **关系**：DWC3 驱动实现了 UDC 框架要求的接口，从而让各种 USB 功能能够运行在 DWC3 硬件上。

想了解如何通过修改 DWC3 驱动来解决特定的 USB 通信问题吗？

