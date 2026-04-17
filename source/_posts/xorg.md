---
title: xorg
date: 2025-03-26 22:10:13
categories:
  - linux
tags:
  - x11
---

## xserver

- x11: x协议的第11个版本，X11只是一套协议，具体实现有各种版本。如 `xserver` `vcxsrc` 等
- x window system：设计用于让多个程序能共享访问同一套硬件。这里的硬件指的是所有的输入设备（键盘，鼠标）和输出设备（显卡，显示器）
- xserver：是实现x window system设计目标的一个计算机程序。统筹管理计算机硬件的输入输出资源的服务器程序。输入包括——键盘，鼠标，触摸屏等。输出包括——屏幕(monitor)，
- rendering/rasterization：渲染和光栅化。一开始X协议一开始定义了一套绘图操作原语。如线条绘制，多边形填充，图片缓冲区拷贝等。但是由于显卡发展太快，导致这一套东西没有及时更新迭代。现代化的客户端程序使用各种客户端侧渲染的库，绕过xserver直接在显卡进行操作。如2D的 `cairo` 3D的 `OpenGL` 渲染。这些库绘制出图片之后可以提交给xserver进行渲染，要不就是绕过xserver通过 `DRI` 进行显卡直通
- display：


### x11绘图过程

绘制一条线的过程可以概括为：
1. 应用程序调用 XDrawLine，通过网络协议发送绘图请求。
2. X 服务器接收请求，进行光栅化，确定要改变哪些像素。
3. 驱动程序将光栅化结果写入显存中的帧缓冲区。
4. 显卡读取帧缓冲区内容，将其显示在屏幕上。


### greeter（登录界面）

1. greeter的启动顺序：【greeter也是gui程序，按理说是要先启动xorg，才能启动greeter】

验证：查看 `/var/log/lightdm/lightdm.log` 中有启动xorg和greeter的时间和pid，这些都能佐证这个启动顺序。lightdm作为systemd服务，首先启动xorg，再启动greeter。


2. 如何自定义greeter？

- 添加 `xxx-greeter.desktop` 文件在 `/usr/share/xgreeters/` 目录下
- 在lightdm配置文件中指定desktop文件名 `/etc/lightdm/lightdm.conf` 


3. 如何编写greeter？

lightdm提供了glib接口，但是有其他语言绑定封装的库可以使用。 qt有 `liblightdm-qt5-3-dev` 。js有 webkit2的封装

https://ssk-wh.github.io/2023/04c447ae7.html

### vnc

[vnc的systemd方案](vnc方案.md)

```bash
# apt install x11vnc

export DISPLAY=:0
x11vnc -rfbport 5901 -shared -ncache 4 -forever -bg -repeat
```

x11vnc是基于 `rdp` 协议来实现远程控制的。不同于tigervnc-standalone-server的方式。tigervnc采用的是重开一个X session会话的方式，这种方式多个用户互相不影响对方的操作



### xcb


### window manager

窗管列表

**x11**

- [lxqt-session/config/windowmanagers.conf at master · lxqt/lxqt-session](https://github.com/lxqt/lxqt-session/blob/master/config/windowmanagers.conf)

**wayland**

- [lxqt-session/config/waylandwindowmanagers.conf at master · lxqt/lxqt-session](https://github.com/lxqt/lxqt-session/blob/master/config/waylandwindowmanagers.conf)

基本原理代码

- [mackstann/tinywm: The tiniest window manager.](https://github.com/mackstann/tinywm)
- [jichu4n/basic_wm: An example basic X11 window manager.](https://github.com/jichu4n/basic_wm/tree/master)



### 小工具

**根据PID激活窗口**

```bash
#!/bin/bash

# 检查是否提供了 PID 参数
if [ -z "$1" ]; then
    echo "用法: $0 <进程PID>"
    echo "示例: $0 12345"
    exit 1
fi

TARGET_PID="$1"

# 查找给定 PID 对应的所有窗口 ID
# 注意：如果一个 PID 有多个窗口，此命令会返回所有它们的 ID
WINDOW_IDS=$(wmctrl -lp | grep " $TARGET_PID " | awk '{print $1}')

if [ -z "$WINDOW_IDS" ]; then
    echo "未找到 PID $TARGET_PID 对应的窗口。"
    exit 1
fi

# 遍历找到的所有窗口 ID，并激活它们
# 在大多数情况下，只有一个主窗口需要激活
for WID in $WINDOW_IDS; do
    echo "正在激活窗口 ID: $WID (PID: $TARGET_PID)"
    wmctrl -i -a "$WID"
    # 如果你只想激活第一个找到的窗口，可以在这里加上 `break`
    # break
done

echo "操作完成。"

```

