---
title: Systemd开关机hook脚本
date: 2026-04-14 14:59:53
categories:
  - linux
tags:
  - linux
---
## systemd不常用命令

```bash
# systemd启动时间分析  
systemd-analyze plot > /tmp/boot.svg  
```

## 关机/重启事件拦截

- `systemd` unit file

```bash
# work.service

[Unit]
Description=MCU开关机任务
DefaultDependencies=no
Before=reboot.target poweroff.target halt.target shutdown.target
After=timers.target

[Service]
Type=oneshot
#WorkingDirectory=/opt/arash/bin
ExecStart=/opt/arash/bin/check.sh
User=root
TimeoutSec=6

[Install]
WantedBy=reboot.target poweroff.target timers.target

```


#### 脚本

```bash

#!/bin/bash

date +"%D %T"
mcu='/opt/arash/bin/MCU'

# 日志文件，用于调试
LOG_FILE="/var/log/mcu.log"

# 使用 systemctl list-jobs 来检查是否有 reboot.target 任务
# grep -q 会在找到匹配项后立即退出，效率很高，并且不产生任何输出
if systemctl list-jobs | grep -q 'reboot.target.*start'; then
    echo "$(date): Reboot detected. Performing reboot-specific tasks..." >> $LOG_FILE
    $mcu reboot
elif systemctl list-jobs | grep -q -E 'poweroff.target.*start|halt.target.*start'; then
    echo "$(date): Shutdown/Power-off detected. Performing shutdown-specific tasks..." >> $LOG_FILE
    $mcu shutdown
elif systemctl list-jobs | grep -q 'setvtrgb.service.*start';then
    echo "$(date): system init detected. Preforming RTC tasks..." >> $LOG_FILE
    $mcu timesync
    timedatectl show
else
    # 理论上不应该发生，但作为备用方案
    echo "$(date): Neither reboot nor poweroff detected in job list. Fallback action." >> $LOG_FILE
    # 记录当前任务列表以供分析
    systemctl list-jobs >> $LOG_FILE
fi

sync

```
