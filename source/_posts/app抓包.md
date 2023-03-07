---
title: app抓包
date: 2022-09-17 17:46:40
categories:
- 爬虫
tags:
- fiddler
- 抓包
- 小工具
---




## 抓包步骤

1. 下载fiddler，一个网络抓包工具
2. 下载nox模拟器，并开启多开模式使用Android 5版本来安装要抓包的应用，Android 6之后，用户添加的ca证书无法生效
3. 设置模拟器中的网络代理，将代理服务器地址和端口设置为fiddler的代理端口和本机ip
4. fiddler中开启https的解密功能，将解密需要的ca证书在nox浏览器中通过代理ip和端口下载并安装
5. 在nox的游戏中心下载xposed模块
6. 下载justtrustme.apk，并在xposed中选中justtrustme。
7. 开启抖音，并点击vlog主的主页中。此时fiddler中会看到一个比较大的包，看response中的json中play\_url。就是需要的视频地址了。
