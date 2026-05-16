---
title: tmux配置与使用
date: 2021-07-30 10:47:52 
categories:
- 工具
tags:
---

## concept
session: a session could include mutil-windows
windows: just the window
panle: a window contains muti-panle

## plugins

在 tmux 中使用插件最正统、最简单的方法是使用 **TPM (Tmux Plugin Manager)**。它就像是 tmux 界的 `vim-plug` 或 `npm`。

以下是配置和使用插件的完整步骤：

### 1. 安装 TPM

首先，你需要将 TPM 的代码克隆到本地配置目录中：

```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

---

### 2. 配置 `~/.tmux.conf`

打开你的配置文件 `vi ~/.tmux.conf`，在文件的**最底部**添加以下内容：

```
# 1. 启用 TPM 插件列表
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# 2. 在这里添加更多你喜欢的插件
# 示例：添加一个著名的配色方案 (Nord)
set -g @plugin "nordtheme/tmux"

# 3. 初始化 TMUX 插件管理器 (这一行必须放在配置文件的最后)
run '~/.tmux/plugins/tpm/tpm'
```

---

### 3. 安装插件

配置好文件后，你需要让 tmux 真正去下载这些插件：

1. 进入 tmux 会话。
    
2. 按下前缀键 **`Ctrl + b`**。
    
3. 按下大写的 **`I`** (Install)。
    
4. tmux 会弹出一个提示，显示正在安装插件，完成后按下 `Enter` 即可。
    

---

### 4. 常用管理快捷键

一旦安装了 TPM，你只需要记住三个组合键：

- **`Prefix + I`**：安装新插件并刷新环境。
    
- **`Prefix + u`**：更新（update）插件。
    
- **`Prefix + Alt + u`**：卸载（uninstall）不再插件列表中的插件。
    

---

### 5. 值得推荐的插件

如果你不知道装什么，可以试试这几个“神器”：

|**插件名称**|**作用**|
|---|---|
|**`tmux-plugins/tmux-resurrect`**|电脑重启后，一键恢复你的 tmux 会话和布局。|
|**`tmux-plugins/tmux-continuum`**|配合 resurrect，每隔15分钟自动保存会话，实现全自动恢复。|
|**`tmux-plugins/tmux-yank`**|让 tmux 的复制粘贴更符合系统剪贴板习惯。|
|**`dracula/tmux`**|一个非常漂亮的、信息丰富的状态栏配色主题。|

**温馨提示**：修改完 `.tmux.conf` 后，别忘了运行 `tmux source ~/.tmux.conf` 或者重启 tmux 进程，这样 TPM 才能正常加载。


### 6. TUI版本

[psmux/Tmux-Plugin-Panel: A full-fledged TUI plugin manager for tmux — the modern alternative to TPM. Browse, install, remove, update plugins and themes from a beautiful terminal interface.](https://github.com/psmux/Tmux-Plugin-Panel)


## usage

```bash

tmux new -s <session name>
tmux new-session -s <session name>
tmux rename-session <new name> # rename当前session

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
c-b space    # 修改当前window中的panel布局
c-b d       # detach当前session
c-b up/down/left/right # foucs panel
c-b [   # pageup/pagedown  `q` to exit page mode

c-b z   # enter / return full screen

c-b (   # 切换到上一个session
c-b )   # 切换到下一个session
c-b L   # 切换到最后一个session
c-b $   # 重命名当前session
c-b ,    # 重命名window
```


## copy-mode

在 `tmux` 中进入复制模式（Copy Mode）并使用鼠标选择文本时，如果直接跨行拖拽，默认会选中**整个终端行的物理宽度**，导致内容跨越到相邻的 Panel（面板）中。

这是因为终端模拟器（如 iTerm2, Alacritty, CMD）通常将屏幕视为一整行，而不知道 `tmux` 在内部进行了垂直分割。

要解决这个问题，有以下几种方案：

---

### 1. 使用 Tmux 原生的“块选择” (最推荐)

如果你开启了 `tmux` 的鼠标支持，可以使用快捷键强制进行 **矩形/列选择**，这样就不会跨越边界。

- **操作方法：** 按住 **`Alt`** (macOS 上通常是 `Option`) 的同时点击并拖拽鼠标。
    
- **原理：** 这会告诉 `tmux` 只选中当前面板内的矩形区域。
    

---

### 2. 临时全屏化当前 Panel (最实用)

这是最简单、最不容易出错的方法。在复制之前，先将当前面板全屏化，复制完后再还原。

1. 将光标移动到目标面板。
    
2. 按下 **`Prefix` + `z`** (Zoom)：当前面板会充满整个屏幕。
    
3. 此时进行复制，由于没有相邻面板，自然不会跨行。
    
4. 复制完成后，再次按下 **`Prefix` + `z`** 还原布局。
    

---

### 3. 使用 `tmux` 内部复制模式 (键盘流)

如果你完全不使用鼠标，而是进入 `copy-mode` 使用键盘操作，`tmux` 会原生限制在当前面板范围内：

1. **`Prefix` + `[`**：进入复制模式。
    
2. 移动光标到起点，按 **`Space`** 开始选中。
    
3. 使用 `hjkl` 移动，你会发现光标无法跳出当前面板边界。
    
4. 按 **`Enter`** 复制，按 **`Prefix` + `]`** 粘贴。
    

---

### 4. 永久配置优化 (针对鼠标用户)

如果你希望鼠标选择默认就更智能，可以在 `~/.tmux.conf` 中添加或确认以下配置：

Bash

```
# 开启鼠标支持
set -g mouse on

# 如果你使用的是 tmux 3.0 或更高版本
# 鼠标拖拽默认应该就是按面板选择的
# 如果无效，可以尝试在拖拽时配合以下按键：
# macOS: 按住 Option 键拖拽
# Linux: 按住 Alt 键拖拽
```

---

### 总结建议

- 如果你只是**临时拷一段话**：用 **`Prefix` + `z`** 缩放面板是最稳妥的，它彻底解决了物理行重合的问题。
    
- 如果你需要**频繁复制多列**：学会按住 **`Alt` / `Option`** 键进行鼠标选择。


你说得对，tmux 默认在 **copy mode（复制模式）** 中会根据 `VISUAL` 或 `EDITOR` 环境变量自动选择使用 Emacs 或 Vi 风格的快捷键。但如果想**强制启用 Vi 风格**或进行更精细的配置，可以通过以下方式：

### 1. 强制设置 copy-mode 使用 Vi 风格
在 `~/.tmux.conf` 中添加：
```tmux
# 设置 copy-mode 使用 vi 快捷键
set -g mode-keys vi

# 同时设置命令行编辑模式也为 vi（可选）
set -g status-keys vi
```
之后重启 tmux 或执行 `tmux source-file ~/.tmux.conf` 生效。

### 2. Vi 风格下常用的 copy-mode 快捷键
进入 copy mode：`Prefix [`（默认 `Prefix` 是 `Ctrl+b`）

| 快捷键 | 作用 |
|--------|------|
| `h` / `j` / `k` / `l` | 左/下/上/右移动光标 |
| `0` / `^` | 移动到行首 / 行首非空白字符 |
| `$` | 移动到行尾 |
| `w` / `b` | 向前/向后移动一个单词 |
| `gg` / `G` | 移动到顶部/底部 |
| `Ctrl+u` / `Ctrl+d` | 向上/向下翻半页 |
| `Ctrl+b` / `Ctrl+f` | 向上/向下翻整页 |
| `/` / `?` | 向前/向后搜索 |
| `n` / `N` | 下一个/上一个匹配项 |
| `Space` | 开始选中文本 |
| `Enter` | 复制选中文本并退出 copy mode |
| `v` | 进入选中模式（可配合方向键扩展选区） |
| `y` | 复制选中文本（不退出 copy mode） |
| `q` | 退出 copy mode |

### 3. 检查当前环境变量是否生效
```bash
echo $EDITOR
echo $VISUAL
```
如果输出包含 `vi`、`vim` 等，tmux 会自动启用 Vi 风格的 copy-mode。如果没有，可以在 shell 配置文件中设置：
```bash
export EDITOR=vim
export VISUAL=vim
```

### 4. 其他相关 Vi 风格配置
```tmux
# 使用 vi 风格的键绑定进行窗格切换（类似 vim 的 Ctrl+w + 方向）
bind -r k select-pane -U
bind -r j select-pane -D
bind -r h select-pane -L
bind -r l select-pane -R

# 或使用 Alt+hjkl（不按 Prefix）
bind -n M-h select-pane -L
bind -n M-j select-pane -D
bind -n M-k select-pane -U
bind -n M-l select-pane -R
```

### 5. 验证当前设置
在 tmux 中执行：
```bash
tmux show -g mode-keys   # 应显示 vi
tmux show -g status-keys # 应显示 vi
```

**总结**：推荐在 `.tmux.conf` 中显式添加 `set -g mode-keys vi`，以确保无论环境变量如何，始终获得一致的 Vi 风格操作体验。

## configure

```bash
# source the tmux config file
# tmux source-file ~/.tmux.conf

# 修改prefix为 '\' 或者 'alt+a'
unbind C-b
set -g prefix M-a
bind M-a send-prefix

# or

unbind C-b
set -g prefix '\'
bind '\' send-prefix


set -g base-index 1 # 设置窗口的起始下标为1
set -g pane-base-index 1 # 设置面板的起始下标为1

set -g status-interval 1 # 状态栏刷新时间
set -g status-justify left # 状态栏列表左对齐

set -wg window-status-format " #I #W " # 状态栏窗口名称格式
set -wg window-status-current-format " #I:#W#F " # 状态栏当前窗口名称格式(#I：序号，#w：窗口名称，#F：间隔符)
set -wg window-status-separator "" # 状态栏窗口名称之间的间隔

set -g default-terminal "screen-256color" # tmux和vim配色冲突


# tmux 插件管理---begin
set -g @plugin 'tmux-plugins/tpm'
#set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'
# 会话保存插件
# prefix+ ctrl+s == 保存会话
# prefix+ ctrl+r == 加载保存的会话

run '~/.tmux/plugins/tpm/tpm'
# tmux 插件管理---end

set-option -g mouse on
set-option -g prefix C-x
unbind ^X
bind -r ^X next-window


bind-key -n M-1 previous-window
bind-key -n M-2 next-window

bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

bind-key q set-option status

```

## reference
[常用快捷键](https://www.cnblogs.com/lizhang4/p/7325086.html)
[官方使用文档](https://tao-of-tmux.readthedocs.io/zh_CN/latest/manuscript/05-session.html#tmux-sessions)
[用鼠标切换窗口/调节分屏大小](https://www.cnblogs.com/bamanzi/p/tmux-mouse-tips.html)
unknown option: mouse-resize-pane错误
set-option -g mouse on
[tmux插件管理器的安装和使用](https://www.cnblogs.com/hongdada/p/13528984.html)
[写的好的个人博客](http://louiszhai.github.io/2017/09/30/tmux/#%E5%AF%BC%E8%AF%BB)

