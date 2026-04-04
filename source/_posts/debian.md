---
title: debian
date: 2025-09-20 15:51:53
categories:
  - linux
tags:
---


记录笔记本安装升级debian系统出现的一些驱动和内核相关的一些问题。


### 网卡

```bash
查看网卡

$ lspci -nnk | grep -i net

01:00.0 Network controller [0280]: Qualcomm Atheros QCA9377 802.11ac Wireless Network Adapter [168c:0042] (rev 31)
```

Device 'Qualcomm Atheros QCA9377 802.11ac Wireless Network Adapter'这个无线网卡，如果要安装这个网卡的驱动在Linux系统上，我需要怎么做？我的内核版本是Linux6.12.38。发行版是debian13。硬件驱动版本和发行版本有关系吗，还是只和内核版本相关？

根据搜索结果，**Qualcomm Atheros QCA9377** 无线网卡在 **Linux 6.12.38 + Debian 13** 环境下**基本可以“开箱即用”**，无需手动编译驱动。但需确认系统已安装 **非自由（non-free）固件包**，因为 QCA9377 的固件属于闭源组件，Debian 默认不自带。

### ❓驱动与“发行版”还是“内核”相关？

- **驱动代码本身**（如 `ath10k_pci`）**由内核提供**，因此**内核版本决定是否支持该芯片**。
    
- **固件文件**（如 `firmware-atheros`）**由发行版打包提供**，因此**发行版决定是否默认包含这些固件**。

> ✅ 结论：  
> **QCA9377 的驱动已内置在 Linux 6.12.38 中**，但 **Debian 13 默认不包含闭源固件**，需手动安装 `firmware-atheros` 包


1. 在能上网的电脑上下载离线包
    
    1. 浏览器打开  
        [https://packages.debian.org/bookworm/firmware-atheros](https://packages.debian.org/bookworm/firmware-atheros)
        
    2. 页面最下方根据自己架构点链接（笔记本一般是 amd64）：  
        “firmware-atheros_20230210-5_all.deb” （版本号可能略不同，用最新的即可）。
        
    3. 将deb安装包使用dpkg安装后重启

```bash
# 验证
ip link

应该能看到 `wlan0` 或 `wlp*` 接口；
```





### 声卡

在从Ubuntu20升级到Ubuntu24之后出现系统无法找到声卡，无法出声音的问题


```bash
排查顺序：

sudo lspci -nn | grep -i audio      # 看 PCI 声卡
lsusb | grep -i audio               # 看 USB 声卡
cat /proc/asound/cards              # 看内核是否建立声卡

```

真实情况：
```

# Intel Jasper Lake（JSL）声卡已经被内核和 SOF 驱动正确识别
$ sudo lspci -nn | grep -i audio
00:1f.3 Audio device [0403]: Intel Corporation Jasper Lake HD Audio [8086:4dc8] (rev 01)

# 但系统里没有 `sof-jsl.ri` 固件，所以 ALSA 最终没注册任何声卡 → `/proc/asound/cards` 为 “no soundcards”。
$ cat /proc/asound/cards
--- no soundcards ---

 sudo dmesg | grep -i sof
[    0.040952] software IO TLB: area num 4.
[    0.358553] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    0.406458] PCI-DMA: Using software bounce buffering for IO (SWIOTLB)
[    0.406459] software IO TLB: mapped [mem 0x0000000069ef9000-0x000000006def9000] (64MB)
[    3.769993] snd_hda_intel 0000:00:1f.3: Digital mics found on Skylake+ platform, using SOF driver
[    4.444635] sof-audio-pci-intel-icl 0000:00:1f.3: enabling device (0000 -> 0002)
[    4.444877] sof-audio-pci-intel-icl 0000:00:1f.3: DSP detected with PCI class/subclass/prog-if 0x040380
[   10.514055] sof-audio-pci-intel-icl 0000:00:1f.3: bound 0000:00:02.0 (ops i915_audio_component_bind_ops [i915])
[   10.522570] sof-audio-pci-intel-icl 0000:00:1f.3: use msi interrupt mode
[   10.561224] sof-audio-pci-intel-icl 0000:00:1f.3: hda codecs found, mask 5
[   10.561233] sof-audio-pci-intel-icl 0000:00:1f.3: using HDA machine driver skl_hda_dsp_generic now
[   10.561235] sof-audio-pci-intel-icl 0000:00:1f.3: BT link detected in NHLT tables: 0x0
[   10.561238] sof-audio-pci-intel-icl 0000:00:1f.3: DMICs detected in NHLT tables: 2
[   10.561498] sof-audio-pci-intel-icl 0000:00:1f.3: firmware: failed to load intel/sof/sof-jsl.ri (-2)
[   10.561505] sof-audio-pci-intel-icl 0000:00:1f.3: firmware: failed to load intel/sof/sof-jsl.ri (-2)
[   10.561511] sof-audio-pci-intel-icl 0000:00:1f.3: firmware: failed to load intel/sof/sof-jsl.ri (-2)
[   10.561515] sof-audio-pci-intel-icl 0000:00:1f.3: SOF firmware and/or topology file not found. ### 这里显示sof-jsl.ri固件缺失
[   10.561538] sof-audio-pci-intel-icl 0000:00:1f.3: Supported default profiles
[   10.561539] sof-audio-pci-intel-icl 0000:00:1f.3: - ipc type 0 (Requested):
[   10.561541] sof-audio-pci-intel-icl 0000:00:1f.3:  Firmware file: intel/sof/sof-jsl.ri
[   10.561542] sof-audio-pci-intel-icl 0000:00:1f.3:  Topology file: intel/sof-tplg/sof-hda-generic-2ch.tplg
[   10.561544] sof-audio-pci-intel-icl 0000:00:1f.3: Check if you have 'sof-firmware' package installed.
[   10.561545] sof-audio-pci-intel-icl 0000:00:1f.3: Optionally it can be manually downloaded from:
[   10.561546] sof-audio-pci-intel-icl 0000:00:1f.3:    https://github.com/thesofproject/sof-bin/
[   10.611924] sof-audio-pci-intel-icl 0000:00:1f.3: error: hda_dsp_core_power_down: timeout on HDA_DSP_REG_ADSPCS read
[   10.611956] sof-audio-pci-intel-icl 0000:00:1f.3: error: dsp core power down fail mask 3: -110
[   13.630021] sof-audio-pci-intel-icl 0000:00:1f.3: error: sof_probe_work failed err: -2
```

先说结论：**就是缺固件**，不是硬件坏。（官方仓库已有包）
```bash
sudo apt install firmware-sof-signed
```

安装后重启，任然显示

```
$  cat /proc/asound/cards
--- no soundcards ---
```


目前已满足两个前提：

1. 固件文件 `sof-jsl.ri` 已经存在（dmesg 不再提示 "firmware not found"）。
    
2. 驱动和拓扑也加载了（dmesg 出现 `sof-hda-generic-2ch.tplg`、`ABI x:y:z` 等字样）。
    

但 **DSP 反复启动失败 → 内核最终放弃注册声卡**，所以 `/proc/asound/cards` 仍然是 **"no soundcards"**。  
这是 **Jasper Lake 笔记本** 的通病：**SOF 固件与内核/BIOS 配合不良**，导致 DSP 无法上电完成 boot。

✅ 一步到位：退回 **传统 HDA 模式**（禁用 SOF）

1. 创建 blacklist 文件

```bash
sudo nano /etc/modprobe.d/sof-blacklist.conf
```

写入：

```
blacklist snd-sof-pci
blacklist snd-sof-intel-hda
blacklist snd-sof-intel-hda-common
```

2. 强制加载传统 HDA 驱动

```bash
sudo nano /etc/modprobe.d/sof-hda.conf
```

写入：`options snd-intel-dspcfg dsp_driver=1`

（1 = 使用 legacy HDA，0 = 自动，3 = 强制 SOF）

3. 更新 initramfs 并重启


```bash
sudo update-initramfs -u
sudo reboot
```

4. 重启后检查

```bash
cat /proc/asound/cards
```

应该立刻出现类似：

`0 [PCH]: HDA-Intel - HDA Intel PCH`


🔍 **原理说明**

- **dsp_driver=1** 让内核跳过 SOF 固件，直接用老牌 `snd_hda_intel` 驱动。
    
- Jasper Lake 的 HDA 控制器本身支持 legacy 模式，99 % 机型功能完整（耳机、喇叭、麦克风、HDMI 音频均可）。
    
- 仅损失 SOF 带来的 **DSP 低功耗/语音唤醒** 等高级特性，对普通播放毫无影响。
    

---

**如果以后想再试 SOF**

删除上述两个 blacklist 文件，把 `dsp_driver=1` 改成 `3` 或删掉，再更新 initramfs 即可回退到 SOF 模式；同时关注内核更新（≥ 6.6 对 JSL 支持更好）。