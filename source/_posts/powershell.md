---
title: powershell
date: 2022-04-07 22:31:08
categories:
  - 工具
tags:
  - 语法
  - 日常使用
  - windows
---


## 远程执行相关

```powershell

$Local_Save_Dir = Get-Location


# --- 交互式获取配置 ---
$Linux_IP = Read-Host "请输入目标 Linux 机器的 IP 地址"

# 简单的 IP 校验：如果输入为空，则退出
if (-not $Linux_IP) {
    Write-Host "错误：未输入 IP 地址，脚本退出。" -ForegroundColor Red
    Pause
    exit
}

# 确保本地存在 plog 脚本以及 pscp.exe/plink.exe
if (-not (Test-Path "./plog")) {
    Write-Error "错误：当前目录下未找到 'plog' 脚本文件！"
    Pause
    exit
}

# --- 1. 将脚本传输到 Linux ---
Write-Host "正在传输脚本到 Linux..." -ForegroundColor Cyan
./pscp.exe -pw ${Password} ./plog "${User}@${Linux_IP}:${Remote_Script_Path}"

# --- 2. 赋予执行权限并运行脚本 ---
# 我们通过 SSH 捕获脚本最后生成的压缩包路径
Write-Host "正在 Linux 上运行脚本..." -ForegroundColor Cyan
$Remote_Command = "chmod +x $Remote_Script_Path && $Remote_Script_Path && ls -t /tmp/LLL_*.tar.gz | head -n 1"
# $Tar_Path = ssh "${User}@${Linux_IP}" "$Remote_Command"
$Tar_Path = ./plink.exe -batch -pw ${Password} "${User}@${Linux_IP}" "$Remote_Command"

# 清理返回字符串中的换行符
$Tar_Path = $Tar_Path.Trim()

# 清理返回字符串中的换行符和潜在的 SSH 警告信息
if ($Tar_Path) {
    $Tar_Path = $Tar_Path.Trim().Split("`n")[-1] # 只取最后一行输出，防止有 SSH 登录横幅干扰
}
```

- `pscp` : (Putty 家族工具)如果你不想配置密钥，可以使用 pscp.exe。它支持 -pw 参数直接传入密码。
- `plink` : 是 PuTTY 家族中的命令行工具，专门用于执行远程命令
	- -batch：禁用交互式提示（如遇到服务器指纹确认会直接报错而不是卡住）。
	- -pw：后面直接跟明文密码。


## 执行策略

`AllSigned`。 要求所有脚本和配置文件都由受信任的发布者签名，包括在本地计算机上编写的脚本。
`Bypass`。 不阻止任何操作，并且没有任何警告或提示。
`Default`。 设置默认执行策略。 Restricted 适用于 Windows 客户端或 RemoteSigned Windows 服务器。
`RemoteSigned`。 要求从 Internet 下载的所有脚本和配置文件都由受信任的发布者签名。 Windows 服务器计算机的默认执行策略。
`Restricted`。 不加载配置文件或运行脚本。 Windows 客户端计算机的默认执行策略。
`Undefined`。 没有为范围设置执行策略。 从组策略未设置的范围中删除分配的执行策略。 如果所有范围内的执行策略为 Undefined，则有效执行策略为 Restricted。
`Unrestricted`。 加载所有配置文件并运行所有脚本。 如果运行从 Internet 下载的未签名脚本，则系统将提示你需要权限才能运行该脚本.

```powershell
在当前 PowerShell 会话中临时允许执行所有脚本
set-executionpolicy Bypass -Scope Process -Force

执行本地脚本
PowerShell -NoProfile -ExecutionPolicy Bypass -File "main.ps1"

永久设置
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## 常见问题

Q: powershell如何alias？
A: 修改用户的配置文件
    Set-ExecutionPolicy RemoteSigned
    C:\Users\Lost\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

