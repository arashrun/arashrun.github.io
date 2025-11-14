---
title: gstreamer
date: 2025-04-01 13:47:20
categories:
  - 音视频
  - 图形学
tags:
  - 音视频
  - gstreamer
---

#### 基本概念

**Pipeline**


**Element**



**Bin** 

bin是一个容器的概念，多个element组合在一起构成的逻辑单元称为bin。


**Pad**

在 GStreamer 中，**pad（ pads，也称为 "端口"）** 是元素（element）之间连接的接口，用于数据的输入和输出。可以把它理解为管道中元素的 "接口"，数据通过 pad 在不同元素之间流动。
- **src pad（源端口）**：元素输出数据的端口（数据离开元素的地方）
- **sink pad（接收端口）**：元素接收数据的端口（数据进入元素的地方）
- **ghost pad（幽灵端口）** 是一种特殊的 pad，它本身不处理数据，而是作为容器元素（bin）内部某个元素 pad 的 "代理" 或 "别名"。
简单说，ghost pad 的作用是：
1. 让容器元素（bin）对外暴露内部元素的 pad，使容器可以像普通元素一样与外部连接
2. 隐藏容器内部的复杂结构，简化管道设计

**Probe**

在 GStreamer 中，**probe（探针）** 是一种用于监控、检查或修改流经 pad 的数据的机制。它允许你在数据通过 pad 时插入自定义逻辑，常用于调试、分析或动态控制媒体流。



**capabilities** 


**interface** 


**Buffer** 

**Bus** 


**Event**


```bash
# 列出你的摄像头设备支持的所有格式、分辨率和帧率
v4l2-ctl --list-formats-ext -d /dev/video12

# 输出gstreamer管道图（apt install graphviz)
export GST_DEBUG_DUMP_DOT_DIR=/tmp/gstvideo
gst-launch-1.0 playbin uri=file:///home/ccls/ds.mp4 audio-sink='pulsesink client-name=uuid'

dot -Tpng /tmp/gstvideo/xxx.dot -o xxx.png


# MPP开启全局AFBC
export GST_MPP_VIDEODEC_DEFAULT_ARM_AFBC=1

#通过fpsdisplaysink获取帧率
GST_DEBUG=fpsdisplaysink:7 gst-play-1.0 --flags=3 --videosink="fpsdisplaysink \
video-sink=xvimagesink signal-fps-measurements=true text-overlay=false  \
sync=false" test.mp4

#信息输出如下
Press 'k' to see a list of keyboard shortcuts.
Now playing /home/cat/test.mp4
0:00:00.104146271  1868   0x55a0d01790 DEBUG         fpsdisplaysink fpsdisplaysink.c:440:fps_display_sink_start:<fpsdisplaysink0> Use text-overlay? 0
Redistribute latency...
0:00:01.670561179  1868   0x7f84044f70 DEBUG         fpsdisplaysink fpsdisplaysink.c:372:display_current_fps:<fpsdisplaysink0> Updated max-fps to 69.251621
0:00:01.671678203  1868   0x7f84044f70 DEBUG         fpsdisplaysink fpsdisplaysink.c:376:display_current_fps:<fpsdisplaysink0> Updated min-fps to 69.251621
0:00:01.673051588  1868   0x7f84044f70 LOG           fpsdisplaysink fpsdisplaysink.c:381:display_current_fps:<fpsdisplaysink0> Signaling measurements: fps:69.251621 droprate:0.000000 avg-fps:69.251621
0:00:02.175685861  1868   0x7f84044f70 DEBUG         fpsdisplaysink fpsdisplaysink.c:372:display_current_fps:<fpsdisplaysink0> Updated max-fps to 69.281343
0:00:02.176746888  1868   0x7f84044f70 LOG           fpsdisplaysink fpsdisplaysink.c:381:display_current_fps:<fpsdisplaysink0> Signaling measurements: fps:69.281343 droprate:0.000000 avg-fps:69.266479
0:00:02.688069779  1868   0x7f84044f70 DEBUG         fpsdisplaysink fpsdisplaysink.c:372:display_current_fps:<fpsdisplaysink0> Updated max-fps to 72.214109
0:00:02.689163762  1868   0x7f84044f70 LOG           fpsdisplaysink fpsdisplaysink.c:381:display_current_fps:<fpsdisplaysink0> Signaling measurements: fps:72.214109 droprate:0.000000 avg-fps:70.258145
0:00:03.193409405  1868   0x7f84044f70 DEBUG         fpsdisplaysink fpsdisplaysink.c:372:display_current_fps:<fpsdisplaysink0> Updated max-fps to 75.213708
0:00:03.194469849  1868   0x7f84044f70 LOG           fpsdisplaysink fpsdisplaysink.c:381:display_current_fps:<fpsdisplaysink0> Signaling measurements: fps:75.213708 droprate:0.000000 avg-fps:71.492593
```

#### 管道控制

**动态修改运行中管道的元素属性**

```bash
# 查看系统中所有运行的 GStreamer 管道：
gst-inspect-1.0 --types=PIPELINE

# 通过 ID 发送控制命令：假设目标管道 ID 为 0x55f5a7b2c3d4
gst-launch-1.0 controller --id=0x55f5a7b2c3d4 send=vol,volume:0.5
```


### 图片的清晰度是由什么决定的？物理屏幕大小和图片分辨率和图片大小这些吗


理解图片清晰度（Clarity）需要区分几个概念，你提到的几个因素都与之相关，但作用方式不同。简单来说，图片的清晰度主要由以下几个核心因素决定：

1.  **图片分辨率 (Image Resolution)**：
    *   **定义**：指图片包含的像素数量，通常表示为“宽度像素数 × 高度像素数”（例如 1920x1080 像素）。
    *   **重要性**：这是决定图片**信息量**和**细节**的基础。分辨率越高，意味着图片包含的像素越多，能够记录和展现的细节就越丰富，这是清晰度的**内在潜力**。一张低分辨率的图片，无论如何放大或在多好的屏幕上显示，其固有的细节缺失是无法弥补的。

2.  **图片质量/压缩程度 (Image Quality/Compression)**：
    *   **定义**：指图片在存储时（尤其是有损压缩格式如 JPEG）保留了多少原始信息。压缩率越高，文件大小（你提到的“图片大小” - 文件体积，单位 KB/MB）越小，但丢失的细节和引入的噪点/色块（压缩失真）就越多。
    *   **重要性**：即使分辨率很高，如果图片经过了严重的有损压缩，细节会模糊，边缘会产生“马赛克”或“晕轮”现象，同样会导致看起来不清晰。因此，**分辨率和压缩质量共同决定了图片的实际细节表现**。

3.  **显示设备的像素密度 (Pixel Density - PPI/DPI)**：
    *   **定义**：指显示设备（如手机屏幕、电脑显示器）或打印输出**每英寸**包含的像素点数量 (Pixels Per Inch / Dots Per Inch)。它由**物理屏幕大小 (Physical Screen Size)** 和 **屏幕分辨率 (Screen Resolution)** 共同决定。
    *   **公式关系**：`PPI = √(屏幕宽度像素² + 屏幕高度像素²) / 屏幕对角线长度（英寸）`
    *   **重要性**：PPI 决定了你在观看屏幕时，像素点有多么“细密”。
        *   **高 PPI**：像素点小而密集，人眼不易察觉单个像素，图像看起来就平滑、锐利，感觉更“清晰”。（例如 Retina 屏）
        *   **低 PPI**：像素点大而稀疏，容易看到“颗粒感”或“锯齿”，图像看起来就不够清晰。
    *   **物理屏幕大小和屏幕分辨率的关系**：
        *   相同屏幕分辨率下，物理尺寸**越小**，PPI **越高**，看起来越清晰。
        *   相同物理尺寸下，屏幕分辨率**越高**，PPI **越高**，看起来越清晰。

4.  **观看距离 (Viewing Distance)**：
    *   **重要性**：人眼的分辨能力是有限的。在较远的距离观看时，即使屏幕的 PPI 不算特别高，人眼也可能无法分辨出单个像素，图像也会感觉足够清晰。反之，即使是高 PPI 的屏幕，如果凑得非常近，也可能开始察觉到像素结构。手机屏幕 PPI 通常很高，因为我们看得近；电视屏幕 PPI 相对较低，因为我们看得远。

5.  **缩放比例 (Scaling)**：
    *   **重要性**：当你查看的图片分辨率与显示区域的像素数不匹配时，系统需要进行缩放。
        *   **放大 (Upscaling)**：将低分辨率图片显示在高分辨率屏幕上或放大查看时，系统需要“猜测”并填充新的像素，这往往会导致模糊或像素化，降低清晰度。
        *   **缩小 (Downscaling)**：将高分辨率图片缩小显示时，通常效果较好，但如果缩小算法不佳或缩小比例过大，也可能丢失一些细节。
        *   **点对点显示 (1:1 Pixel Mapping)**：当图片的一个像素正好对应屏幕上的一个像素时，能最真实地反映图片本身的清晰度（受限于屏幕 PPI）。

**总结一下你提到的几个概念：**

*   **图片分辨率 (Image Resolution)**：决定了图片**内在**的细节潜力。是清晰度的基础。
*   **物理屏幕大小 (Physical Screen Size)**：**结合屏幕分辨率**一起决定了显示设备的**像素密度 (PPI)**，影响**观看时**的清晰感。
*   **图片大小 (Image Size - 指文件大小 KB/MB)**：通常与分辨率和压缩质量相关。高分辨率、低压缩的图片文件更大，往往更清晰，但文件大小本身**不是**清晰度的直接衡量标准，只是一个**间接指标**。一张高分辨率但压缩很差的图片，文件可能依然不小，但看起来模糊。

**因此，一张图片最终看起来是否清晰，是 图片自身质量（分辨率、压缩） 和 观看条件（显示设备 PPI、观看距离、是否缩放） 共同作用的结果。** 要想获得最佳清晰度体验，理想情况是：高分辨率、高质量（低压缩）的图片，在高 PPI 的屏幕上，以合适的尺寸（最好是 1:1 或适当缩小）和距离观看。