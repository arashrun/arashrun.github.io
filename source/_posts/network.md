## 计算机网络

- 全球asn（as编号）查询	
  http://as.chacuo.net
  https://tools.ipip.net/as.php
  https://www.iamle.com/archives/1961.html

- 路由协议：运行在路由器上的协议，该协议用来确定数据包如何到达目标路径。网关是路由器的旧称。

- 根据路由区域大小，路由协议分为两类：
  在一个as（自治系统）内的路由协议称为   **内部网关协议**  （这里的内部指的是在同一个as中），而各个as之间的路由协议称为   **外部网关协议**

  ​	内部网关协议：
     ​	rip-1
     ​	rip-2
     ​	igrp		上三种使用距离向量算法
     ​	eigrp
     ​	is-is
     ​	ospf		上两种使用链路状态算法

  ​	外部网关协议：
     ​	egp
     ​	bgp


## 字节序和大小端

## udp
不可靠：
无连接：

## tcp三次握手，四次挥手




### 获取IPV6地址

1. 进入路由器，找到ipv6的支持页面，配置路由器的DNS地址为ipv6的公共DNS服务器 `240c::6666` 然后设置Lan模式为桥模式，这样路由器就只相当于一个桥的作用，因此会给Lan（局域网）范围内的主机分配ipv6地址
2. 要保证 `ipconfig` 出来的地址出现 `temporary ipv6 address` 才算分配到了ipv6地址
![](/images/ipv6.png)
3. 通过网站 [IPv6 test - IPv6/4 connectivity and speed test (ipv6-test.com)](https://ipv6-test.com/) 进行ipv6支持性测试



