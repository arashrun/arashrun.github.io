---
title: vps
date: 2021-03-12 10-21-20
categories:
  - 工具
tags:
  - vps
  - 加密
---

[libevent2](_posts/libevent2.md)

## vps 管理

**设置使用密钥登录**

1. 在客户端使用ssh-keygen的非对称加密算法，生成公私钥(id_rsa--私钥,id_rsa.pub--公钥）
2. 将公钥(id_rsa.pub)添加到vps的.ssh/authorized_keys中(私钥很重要，是密钥登录的核心保存好)
3. 确保以下的文件权限(authorized_keys--600 ~/.ssh--700)
4. 设置服务器sshd配置如下:
```shell
#/etc/ssh/sshd_config

# 使用公钥认证的方式登录
PubkeyAuthentication yes

# 禁止使用密码登录的方式
PasswordAuthentication no
```
5. 修改sshd服务的默认端口为10086
```shell
#/etc/ssh/sshd_config
Port 10086
```
6. 设置防火墙开放10086端口
```bash
# 查看防火墙是否开启
firewall-cmd --state

# 添加规则
firewall-cmd --permanent --add-port=10086/tcp

# 确认
firewall-cmd --list-all
```
6. 重启sshd服务
service sshd restart



### vps测评和推荐网站


#### 国内站点

[主机之家测评- 专注分享便宜vps - 国外vps - 国外服务器 - 国外主机 - 测评及优惠码 (liuzhanwu.cn)](https://www.liuzhanwu.cn/)



