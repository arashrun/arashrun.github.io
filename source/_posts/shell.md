---
title: shell
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
- 语法
- linux
---

## 语法

1. 判断是否为数字
```shell
if [ "$1" -gt 0 ] 2>/dev/null;then
    $port = $1
else
    echo "Usage: $0 <port number>"
    exit -1
fi
```

2. 函数声明方式
```shell
function name(){

}
```
3. 查找进程pid
```shell
pgrep -f progressname
pkill -f progressname //kill it after find it

```



## 常用命令

1. sed

