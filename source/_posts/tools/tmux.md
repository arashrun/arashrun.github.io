---
title: tmux配置与使用
date: 2021-07-30 10:47:52 
---

## concept
session: a session could include mutil-windows
windows: just the window
panle: a window contains muti-panle

## install 
sudo apt install tmux -y

## usage
tmux new -s <session name>
tmux new-session -s <session name>

tmux ls
tmux kill-
tmux list-
tmux attach-

tmux a -t <session name> # 从新进入临时退出的tmux会话


c-b %       # verhicel
c-b "
c-b :       #在tmux中输入命令
c-b c       #创建新window
c-b &       #关闭当前window
c-b x       #关闭当前panel
c-b space
c-b d       # 临时退出tmux
c-b up/down/left/right # foucs windows
c-b [   # pageup/pagedown  `q` to exit page mode

c-b z   # enter / return full screen

c-b (   # 切换到上一个session
c-b )   # 切换到下一个session
c-b L   # 切换到最后一个session


## reference
[常用快捷键](https://www.cnblogs.com/lizhang4/p/7325086.html)
[官方使用文档](https://tao-of-tmux.readthedocs.io/zh_CN/latest/manuscript/05-session.html#tmux-sessions)
[用鼠标切换窗口/调节分屏大小](https://www.cnblogs.com/bamanzi/p/tmux-mouse-tips.html)
unknown option: mouse-resize-pane错误
set-option -g mouse on
[tmux插件管理器的安装和使用](https://www.cnblogs.com/hongdada/p/13528984.html)
[写的好的个人博客](http://louiszhai.github.io/2017/09/30/tmux/#%E5%AF%BC%E8%AF%BB)

