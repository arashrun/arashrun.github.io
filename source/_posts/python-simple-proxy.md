---
title: python实现proxy
date: 2023-04-06 09:09:18
categories:
- python
tags:
- proxy
- network
---


### PAC

PAC（proxy auto-configuration）文件是用来自动配置代理的一个JavaScript文件，该文件中会实现一个 `FindProxyForURL` 方法，用于返回访问请求url中的host是否需要通过代理，该函数返回一个字符串。例如如下都是正确的返回值 [return value](https://developer.mozilla.org/en-US/docs/Web/HTTP/Proxy_servers_and_tunneling/Proxy_Auto-Configuration_PAC_file#return_value_format) 。

![](/images/pac_return.png)

> [!note]
> 
> 可以使用[pactester](https://github.com/manugarg/pacparser)，来对pac文件进行测试。
> eg: 
> 	pactester -p pac.txt -u https://www.google.com



### 流程

1. 配置windows代理使用自动配置脚本，在脚本地址中填写相应的pac文件获取途径，注意不能是本地路劲。

> [!note]
> 
> [在 Windows 中使用代理服务器](https://support.microsoft.com/zh-cn/windows/%E5%9C%A8-windows-%E4%B8%AD%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8-03096c53-0554-4ffe-b6ab-8b1deee8dae1) 有三种方式。其中自动的方式是通过WPAD协议来实现的 [WinHTTP AutoProxy 支持 - Win32 apps | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/win32/winhttp/winhttp-autoproxy-support#overview-of-autoproxy) 。


![](/images/auto_proxy.png)

2. client端代码

```python
import http.server as hs
from multiprocessing import Pipe, Process
import asyncore
from multiprocessing.connection import PipeConnection

IP = 'localhost'
PORT = 8989
PAC = ''

class RemoteServer(asyncore.dispatcher):
    def __init__(self, pipe:PipeConnection) -> None:
        super().__init__()
        self.pipe = pipe
        self.create_socket()
        self.connect((IP, PORT))
    # 对请求加密
    def crypto(self, data):
        return b''
	# 加密传输
    def writable(self) -> bool:
        if self.pipe.readable:
            return True
        return False

    def handle_write(self) -> None:
        data = self.pipe.recv()
        data = self.crypto(data)
        self.send(data)
        

class HttpRequestHandler(hs.BaseHTTPRequestHandler):
    def do_GET(self):
	    # 处理pac文件请求
        if self.path[:4] == '/pac':
            self.send_response(200)
            self.send_header('Content-type', 'application/x-ns-proxy-autoconfig')
            self.end_headers()
            with open(PAC, 'rb') as f:
                self.wfile.write(f.read())
            print(self.path[:4], "pac文件发送完成")
        # 其他需要走代理的get请求
        else:
            # 代理转发
            parent_conn.send(self.headers.as_string())
        return

    def do_CONNECT(self):
        print(self.path)
        parent_conn.send(self.headers.as_string())

def ConnectToRemoteServer(conn):
    rs = RemoteServer(conn)
    asyncore.loop()

if __name__ == '__main__':
    parent_conn, child_conn = Pipe()
    p = Process(target=ConnectToRemoteServer, args=(child_conn,))
    p.start()
    addr = ('127.0.0.1', 1080)
    httpd = hs.ThreadingHTTPServer(addr, HttpRequestHandler)
    httpd.serve_forever()
    p.join()
```

