---
title: vnc方案
date: 2026-04-03 14:13:14
categories:
  - 工具
  - linux
tags:
  - 工具
---
将 `x11vnc` 注册为 `systemd` 服务是实现 Linux 远程桌面自动启动和故障自恢复的标准做法。按照以下步骤操作，可以确保服务带密码运行且在崩溃后自动重启。

---

### 第一步：创建密码文件

`x11vnc` 建议使用经过加密的密码文件，而不是在命令行中明文暴露。

1. 创建存储路径：
```bash
sudo mkdir -p /etc/x11vnc
```
2. 生成加密后的密码文件（输入两次 `qwer1234`）：
```bash
sudo x11vnc -storepasswd qwer1234 /etc/x11vnc/vncpwd
```
2. 确保权限安全：
```bash
sudo chmod 600 /etc/x11vnc/vncpwd
```
---

### 第二步：编写 Systemd 服务文件

创建一个新的服务配置文件：
```bash
sudo vim /etc/systemd/system/x11vnc.service
```

将以下内容粘贴进去。**请注意：** 如果你使用的是 Ubuntu/Debian 且有图形登录界面，`-auth` 路径通常是关键。
```ini

[Unit]  
Description=X11VNC Remote Desktop Service  
After=multi-user.target network.target display-manager.service  
​  
[Service]  
Type=simple  
# 核心启动命令  
# -forever: 客户端断开后不关闭服务  
# -rfbauth: 指定密码文件路径  
# -display :0: 绑定到本地第一个显示器  
# -auth guess: 自动探测 X11 授权文件（解决权限问题） export XAUTHORITY=/var/run/lightdm/root/:0  
ExecStart=/usr/bin/x11vnc -forever -display :0 -rfbauth /etc/x11vnc/vncpwd -auth /var/run/lightdm/root/:0 -noxdamage -shared -ncache 4 -rfbport 5901  
​  
# 守护进程设置  
Restart=on-failure  
RestartSec=5  
​  
[Install]  
WantedBy=multi-user.target
```
---

### 第三步：启动并启用服务

执行以下命令加载配置并运行：
```bash
1. 重载配置
   sudo systemctl daemon-reload
2. 设置开机自启
   sudo systemctl enable x11vnc
3. 立即启动
   sudo systemctl start x11vnc
4. 检查状态
   sudo systemctl status x11vnc
```

---

### 关键点解析 (💡 避坑指南)

- **关于 `-auth guess`**：这是最省心的写法。如果服务启动报错 `Empty password` 或 `Cannot open display`，通常是因为它找不到 X11 的授权文件（MIT-MAGIC-COOKIE）。如果 `guess` 失败，你可能需要手动指定，例如：
	- SDDM (KDE): `-auth /var/run/sddm/*`
	- LightDM: `-auth /var/run/lightdm/root/:0`
- **关于守护运行**：`Restart=on-failure` 确保了如果 `x11vnc` 因为 X11 会话重启或程序崩溃而退出，`systemd` 会在 5 秒后自动将其拉起。
- **防火墙**：确保你的服务器开启了 **5900** 端口。

**如果您在启动后发现只能看到登录界面而无法进入桌面，或者遇到“Permission Denied”，通常需要调整服务中的 `User=root` 设置。需要我帮您针对特定的桌面管理器（如 GNOME 或 KDE）进行微调吗？**