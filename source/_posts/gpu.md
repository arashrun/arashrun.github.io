---
title: gpu
date: 2025-05-05 12:46:37
categories:
  - 图形学
tags:
  - gpu
  - 音视频
  - opengl
  - mesa
  - x11
  - glx
---

对于有显示器能现实画面的机器，都是存在显卡的。


### 查看gpu型号

```shell
# 方式1
lspci -nn |grep  -Ei 'VGA|DISPLAY'

00:02.0 VGA compatible controller [0300]: Intel Corporation Device [8086:4e55] (rev 01)

VGA 兼容控制器：是计算机硬件中负责生成和输出视频信号的核心组件，属于 **PCI 设备分类**中的一类（类别代码为 `0300`）。它是操作系统对显示适配器（显卡）的一种标准化识别名称，用于兼容传统 VGA（Video Graphics Array）显示标准，同时支持现代图形接口（如 HDMI、DisplayPort 等）
在 PCI 设备列表中（如通过 `lspci` 命令查看），所有显示控制器（包括集成显卡、独立显卡）都会被归类为 **VGA 兼容控制器**，这是 PCI 规范定义的统一类别名称。具体型号和厂商信息需通过后面的详细描述识别

# hardinfo
sudo apt install hardinfo

```

上面的 `Intel Corporation Device [8086:4e55] (rev 01)` 就是代表是intel显卡，根据intel官方链接说明。 `4e55` 代表的PCI设备ID对应的产品是 

| PCI IDs | Name                | 架构  | 代号          | linux | EU运算单元 |
| ------- | ------------------- | --- | ----------- | ----- | ------ |
| 4E55    | Intel® UHD Graphics | Xe  | Jasper Lake | 5.15  | 16     |


### 查看gpu信息

```shell

# amd
radeontop

# intel
intel_gpu_top

# nvida
nvida-smi

```


### 查看opengl版本信息

```shell
glxinfo |grep -i opengl

# 查看gpu驱动状态。如果输出llvmpipe就代表是通过软件进行渲染的，gpu驱动没有正确驱动，需要安装对应显卡的驱动。
glxinfo | grep "OpenGL renderer"

OpenGL renderer string: llvmpipe (LLVM 12.0.0, 128 bits)

# change the gl version
echo 'export MESA_GL_VERSION_OVERRIDE=4.5' >> ~/.bashrc
```



### 根据gpu型号查找相关驱动


例如上面的 `4E55` 这个gpu设备，最小支持的内核版本是5.15。而Ubuntu20默认的内核版本是5.4.0。因此需要手动升级内核才能正确驱动这个GPU，发挥硬件能力

```shell
sudo apt install linux-generic-hwe-20.04

```

HWE（Hardware Enablement）内核包

HWE 内核的主要特点包括：

- **全面的硬件支持**：通过定期更新，确保与最新硬件完美兼容。
- **卓越的性能表现**：经过精心优化，能显著提升系统运行效率。
- **强化的安全防护**：通过定期修复和更新，全方位加固系统安全。
- **出色的稳定性**：在引入最新功能的同时，保持 LTS 版本的可靠性。

HWE 内核会更新到较新的内核版本，为新硬件提供更好的支持。在长期支持（LTS）版本的 Ubuntu 系统中，HWE 内核能让用户在不升级到新版本 Ubuntu 的情况下，使用到较新内核的特性和硬件支持


