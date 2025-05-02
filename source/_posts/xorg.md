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



### greeter（登录界面）

1. greeter的启动顺序：【greeter也是gui程序，按理说是要先启动xorg，才能启动greeter】

验证：查看 `/var/log/lightdm/lightdm.log` 中有启动xorg和greeter的时间和pid，这些都能佐证这个启动顺序。lightdm作为systemd服务，首先启动xorg，再启动greeter。


2. 如何自定义greeter？

- 添加 `xxx-greeter.desktop` 文件在 `/usr/share/xgreeters/` 目录下
- 在lightdm配置文件中指定desktop文件名 `/etc/lightdm/lightdm.conf` 


3. 如何编写greeter？

lightdm提供了glib接口，但是有其他语言绑定封装的库可以使用。 qt有 `liblightdm-qt5-3-dev` 。js有 webkit2的封装


### vnc

```bash
# apt install x11vnc

export DISPLAY=:0
x11vnc -rfbport 5901 -shared -ncache 4 -forever -bg -repeat
```

x11vnc是基于 `rdp` 协议来实现远程控制的。不同于tigervnc-standalone-server的方式。tigervnc采用的是重开一个X session会话的方式，这种方式多个用户互相不影响对方的操作



### xcb





