---
title: Docker
date: 2023-01-3 22:16
categories:
- 工具
tags:
- docker
---


### docker学习文档

> [Dockerfile reference | Docker Documentation](https://docs.docker.com/engine/reference/builder/)
> [Docker Documentation | Docker Documentation](https://docs.docker.com/)


## 目录
---
是什么
参考文档
	docker命令行参考
	API参考
	Dockerfile参考
	Compose file参考
	驱动和规范文档



#### install(on centos7)
1. curl -sSL https://get.daocloud.io/docker | sh

2. systemctl start docker

3. docker run hello-world

#### 基本原理

```mermaid
sequenceDiagram

participant cl as DockerClient
participant sv as DocerDaemon
participant hb as DockerHub

cl->>sv:contact to 
sv->>hb:pull image
hb-->>sv:ret image
sv->>sv:create container && running output
sv->>cl:streamed that output
```

#### 配置国内源

1. 创建或修改 /etc/docker/daemon.json 文件，修改为如下形式 

   ```json
   {    
       "registry-mirrors" : 
       			[    
       				"https://registry.docker-cn.com",   
       				"https://docker.mirrors.ustc.edu.cn",    
       				"http://hub-mirror.c.163.com",    
       				"https://cr.console.aliyun.com/"  
   				] 
   }
   ```

2. 重启docker服务

   systemctl restart docker



#### 常用命令

**容器相关**

```bash
1. 为client创建一个伪终端
   docker run -it 交互式运行
			<镜像名> 
			--name <容器名称>
			-p <host_port>:<container_port>
			-d 后台运行容器
			-v <host_dir>:<container_dir> 将主机目录映射到容器内指定目录
			/bin/bash 
			
2. 重命名容器
   docker rename <old_name/container_id> <new_name>
   
3. 查看所有的容器信息
   docker ps -a
```


3. 不使用缓存构建镜像
	docker build -t xxx --no-cache .

#### Dockerfile

