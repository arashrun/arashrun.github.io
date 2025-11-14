---
title: PluseAudio
date: 2024-12-25 17:05:24
categories:
  - linux
tags:
  - 框架
---

[PulseAudio - ArchWiki](https://wiki.archlinux.org/title/PulseAudio#default.pa)

[PulseAudio under the hood](https://gavv.net/articles/pulseaudio-under-the-hood/)




```shell
 ~/.config/pulse/default.pa
 # 加载网络模块
.include /etc/pulse/default.pa
load-module module-native-protocol-tcp auth-anonymous=1


# 客户端网络访问
env PULSE_SERVER=172.16.14.248 ./pavucontrol-qt
```


**切换到Line-In**

1. 将Line-In设备作为默认sources
2. 加载回环模块 `module-loopback` 

```shell
[load]
pactl load-module module-loopback source=<source_name> sink=<sink_name>

[额外参数]
- `latency_msec=<value>`: 设置延迟（毫秒）。
- `sink_input_properties=<properties>`: 定义额外的属性，比如音频流名称

[info]
pactl list modules short

[unload]
pactl unload-module <module_id>

eg:
pactl load-module module-loopback source=alsa_input.pci-0000_00_1f.3.analog-stereo sink=alsa_output.pci-0000_00_1f.3.analog-stereo
```


3. 对其他音频流(sink-sources)各种类型[event,video,audio]静音mute,或只保留指定的类型。 `media.role` 既有内置类型，也可以自定义

> sink-input-by-media-role:event
> event: 系统音效
> video: 视频类型
> audio：音频类型
> phone: 手机
> game：游戏




**切换到系统音源**

1. 卸载回环模块
2. 取消静音


**创建虚拟sink设备，并重定向到实际的sink设备**

```
1. 创建虚拟sink

pactl load-module module-null-sink sink_name=alsa_output.platform-LineOut.stereo-fallback sink_properties=device.description="线路输出"

2. 重定向音频流

pactl load-module module-loopback source=LineOut.monitor sink=alsa_output.pci-0000_00_1b.0.analog-stereo


======逆过程=====

1. 取消重定向
2. 删除虚拟sink
```


**修改设备描述**

参考：[pulse-cli-syntax(5) — Arch manual pages](https://man.archlinux.org/man/pulse-cli-syntax.5#NAME)


```bash
修改/etc/pulse/default.pa，添加如下命令，最后重启音频服务

update-sink-proplist alsa_output.platform-hdmi0-sound.stereo-fallback device.description="我的hdmi声卡"

systemctl --user restart pulseaudio.service
```





### Term

- card
一个card代表物理或模拟的音频IC，一个声卡通常包含多个 `port` 和多个可用的 `profile` 

- profile
由于声卡有多个端口，因此可以按需启用相关功能组合。profile就是声卡上的一组功能组合

- port
端口。直接可见的可连接线缆的插孔。一个声卡通常可以连接和处理多个输入输出设备。