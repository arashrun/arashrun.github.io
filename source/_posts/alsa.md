---
title: alsa
date: 2026-04-18 09:07:59
categories:
  - linux
tags:
  - 音视频
---
[ALSA project - the C library reference: Index, Preamble and License](https://www.alsa-project.org/alsa-doc/alsa-lib/index.html) 


## 应用层API

- primitive control
	- base api
	- plugins
	- high level api
- pcm
	- pcm api [alsa-api](alsa-api.md)
	- plugins [pcm_plugins](pcm_plugins.md) 
	- external plugins
	- external control plugins
- rawmidi
- use case [ucm_config](ucm_config.md)
- alsa topology


## 内核层API


## terms

- primitive control：
- pcm:
- sample:【采样】 一个数值
- frames:【帧】一个时间点根据转换器（ADC）的数量采集到的sample集合.单声道frame(v1),多声道frame(v1,v2,v3)
- stream: 连续时间内的sample序列
- chunk: 现代声卡允许控制传输时间周期,chunk代表一个时间周期内的一段stream
- device：【设备】可以是物理的也可以是虚拟的
- use case:
- rawmidi
- sequencer:
- dsp topology:



在音频架构和 Linux 音频子系统（如 ALSA - Advanced Linux Sound Architecture）的语境下，这些术语构成了音频数据传输和处理的核心框架。

我们可以把这些概念想象成一个“录音棚”的不同组成部分：

---

### 1. Primitive Control (原始控制)

这是音频接口中**最底层的控制接口**。

- **解释**：它直接对应硬件寄存器的操作。主要用于控制非流媒体的参数，比如开关（Mute）、增益（Volume）、采样率选择或路由切换。
    
- **类比**：就像调音台上的物理旋钮和电推子，你直接拨动它们来改变电路的状态。
    

### 2. PCM (Pulse Code Modulation - 脉冲编码调制)

这是数字音频的**标准表示方法**。

- **解释**：PCM 接口负责处理“数字音频流”。它将模拟信号在时间上进行采样，并在幅度上进行量化。在 ALSA 中，PCM 设备用于播放（Playback）和录音（Capture）。
    
- **核心参数**：采样率（如 44.1kHz）、位深（如 16bit）、声道数（如 Stereo）。
    
- **类比**：这是录音棚里的磁带或数字音频文件，是真正的声音数据流。
    

---

### 3. Use Case (使用场景 / UCM)

在复杂的现代音频系统中（如手机或电脑），这通常指 **UCM (Use Case Manager)**。

- **解释**：它是一套配置机制，用于根据用户的行为自动切换音频路由。例如，“通话场景”需要开启听筒和降噪麦克风，而“多媒体播放场景”则需要切换到扬声器并调整 EQ。
    
- **作用**：它避免了应用层直接去操作底层的 Primitive Control，而是通过高层逻辑（如 "HiFi", "Phone Call"）一键配置整个音频链路。
    

### 4. RawMidi (原始 MIDI)

处理**乐器数字化接口**数据。

- **解释**：RawMidi 提供对硬件 MIDI 端口（5 针 DIN 或 USB MIDI）的直接访问。它传输的不是声音，而是指令（比如“按下 C4 键，力度为 80”）。
    
- **特点**：它是一种字节流，实时性要求极高。
    
- **类比**：就像一份乐谱，告诉乐器该怎么弹，但乐谱本身不发声。
    

---

### 5. Sequencer (音序器)

比 RawMidi 更高级的 **MIDI 管理层**。

- **解释**：ALSA Sequencer 不仅能传输 MIDI，还能管理定时（Timing）和调度（Scheduling）。它允许不同的软件或硬件客户端相互连接，自动处理事件的同步。
    
- **区别**：RawMidi 是点对点的原始传输，而 Sequencer 是一个复杂的网络，可以处理“在第 5 秒播放这个音符”这样的逻辑。
    

### 6. DSP Topology (DSP 拓扑结构)

描述 **数字信号处理器内部的逻辑连接**。

- **解释**：现代音频芯片（Codec 或专用 DSP）内部非常复杂。Topology 描述了音频数据进入 DSP 后，经过哪些算法模块（如 Gain -> EQ -> Mixer -> DRC），以及这些模块是如何串联或并联的。
    
- **意义**：通过拓扑文件，固件可以动态地告诉驱动程序：这个 DSP 内部长什么样，有哪些控制点可以暴露给用户。
    

---

### 总结对照表

|**术语**|**处理对象**|**核心功能**|
|---|---|---|
|**Primitive Control**|硬件开关/旋钮|调节参数、静音、增益|
|**PCM**|数字音频采样数据|声音的播放与录制|
|**Use Case**|场景逻辑配置|自动化管理音频路径 (如通话/音乐)|
|**RawMidi**|原始字节指令|连接外部 MIDI 设备|
|**Sequencer**|MIDI 事件与时间|处理音乐创作中的同步与调度|
|**DSP Topology**|内部算法路径|定义 DSP 内部的音频处理流水线|

你想了解这些概念是为了进行音频驱动开发，还是在调试 Linux 系统的音频配置？


## amixer命令

```bash
$ amixer contents -c 1
numid=9,iface=MIXER,name='I2STDM Digital Loopback Mode'
  ; type=ENUMERATED,access=rw------,values=1,items=4
  ; Item #0 'Disabled'
  ; Item #1 'Mode1'
  ; Item #2 'Mode2'
  ; Item #3 'Mode2 Swap'
  : values=0
这里的输出我看不懂，能逐行解释一下吗？分号后面和冒号后面
```


这对 `amixer` 的输出是 ALSA 驱动中最核心的调试信息。它描述了一个混音器控件（Control Element）的属性和当前状态。

为了让你看懂，我们将这几行拆开，逐一解释其含义：

### 第一行：基本属性

`numid=9,iface=MIXER,name='I2STDM Digital Loopback Mode'`

- **numid=9**: 这是该控件在内核中的**唯一数字 ID**。你也可以通过 `amixer cset numid=9 1` 来直接修改它。
    
- **iface=MIXER**: 属于 **MIXER 接口**（如前所述，表示这是一个调音/路由相关的控件）。
    
- **name='...'**: 控件的**人类可读名称**。在 PulseAudio 的 Path 文件中引用时，通常就是用这个名字。
    

---

### 第二行（分号后）：数据类型与权限

`; type=ENUMERATED,access=rw------,values=1,items=4`

- **type=ENUMERATED**: **枚举类型**。意味着这个控件不是连续的滑块（如音量），而是像“旋钮”一样，只有几个固定的档位可以选。
    
- **access=rw------**: **访问权限**。
    
    - `r`: 可读（Readable）。
        
    - `w`: 可写（Writable）。
        
    - 后面的短横线表示没有其他特殊权限（如通知、锁定等）。
        
- **values=1**: 表示该控件包含 **1 个数值**。如果是立体声音量，这里通常是 `values=2`。
    
- **items=4**: 表示该枚举控件共有 **4 个可选项**。
    

---

### 第三至六行（分号后）：选项列表

`; Item #0 'Disabled'`

`; Item #1 'Mode1'` ...

- 这些是该枚举控件的具体**选项名称**。
    
- 每一个 `Item` 对应一个索引数字。比如当你把值设为 `0` 时，硬件就处于 `Disabled` 状态。
    

---

### 最后一行（冒号后）：当前状态值

`: values=0`

- 这反映了**硬件当前的实际设置**。
    
- 这里的 `0` 对应上面的 `Item #0`。
    
- **结论**：目前的“数字回环模式”处于 **Disabled（禁用）** 状态。
    

---

### 总结图解

为了更直观地理解，你可以把这个输出想象成一个带有 4 个档位的物理拨码开关：

- **分号 ( ; ) 后面**的内容是“说明书”：它告诉你这个开关是什么型号、有几个档位、每个档位叫什么名字。
    
- **冒号 ( : ) 后面**的内容是“刻度位置”：它告诉你现在开关指在哪个档位上。
    

### 常见的其他属性（补充）

如果是一个音量控件，你还会看到不同的信息：

- **type=INTEGER**: 整数类型（通常用于音量）。
    
- **min=0, max=255**: 告诉你音量滑块的范围。
    
- **step=1**: 调节的步进大小。
    

你的这个硬件非常简洁，全都是基于这种“开关”式的枚举控制，这在数字总线（I2S/TDM）的路由配置中非常典型。