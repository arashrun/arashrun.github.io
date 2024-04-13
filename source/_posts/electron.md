---
title: electron
date: 2023-04-19 10:54:25
categories:
- web
tags:
- electron
- 桌面应用
---


### 安装



```powershell

//直接安装会出现网络错误
❯❯ electron   10:46 npm install electron --save-dev
npm ERR! code 1
npm ERR! path D:\k-s\private\electron\node_modules\electron
npm ERR! command failed
npm ERR! command C:\WINDOWS\system32\cmd.exe /d /s /c node install.js
npm ERR! RequestError: connect ETIMEDOUT 20.205.243.166:443
npm ERR!     at ClientRequest.<anonymous> (D:\k-s\private\electron\node_modules\got\dist\source\core\index.js:970:111)
npm ERR!     at Object.onceWrapper (node:events:640:26)
npm ERR!     at ClientRequest.emit (node:events:532:35)
npm ERR!     at ClientRequest.origin.emit (D:\k-s\private\electron\node_modules\@szmarczak\http-timer\dist\source\index.js:43:20)
npm ERR!     at TLSSocket.socketErrorListener (node:_http_client:442:9)
npm ERR!     at TLSSocket.emit (node:events:520:28)
npm ERR!     at emitErrorNT (node:internal/streams/destroy:157:8)
npm ERR!     at emitErrorCloseNT (node:internal/streams/destroy:122:3)
npm ERR!     at processTicksAndRejections (node:internal/process/task_queues:83:21)
npm ERR!     at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1157:16)

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\ming\AppData\Local\npm-cache\_logs\2023-04-19T02_46_23_786Z-debug-0.log

//解决办法：设置mirror
❯❯ electron   10:47 npm config set electron_mirror "https://npm.taobao.org/mirrors/electron/"

//再次执行
❯❯ electron  10:52 npm install electron --save-dev

added 71 packages, and audited 72 packages in 59s

17 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

```


### 运行

```powershell
npm run start
```


### 打包

```powershell
- 在项目中安装打包工具 electron-forge
npm install --save-dev @electron-forge/cli
- 将项目导入electron-forge
npx electron-forge import
- 执行打包
npm run make
```



### 项目

- [mdviewer: 简单的markdown浏览应用，不支持编辑，适合于简单的单markdown文件浏览 (gitee.com)](https://gitee.com/arashrun/mdviewer)




### 官方地址



[使用 Node.js 读取文件 (nodejs.cn)](http://dev.nodejs.cn/learn/reading-files-with-nodejs)
[Introduction | Electron (electronjs.org)](https://www.electronjs.org/docs/latest/)

[CLI - Electron Forge](https://www.electronforge.io/cli#start)
