---
title: archlinux
date: 2022-04-07 22:32:19
categories:
- linux
tags: 
- linux
---



## 安装流程

```bash
0.References
https://www.jianshu.com/p/7c78dc4c53e5
https://zhuanlan.zhihu.com/p/202914804

0. 下载镜像，并制作镜像盘
制作iso镜像工具：https://www.balena.io/etcher/

1.网络连接检测
(使用iwctl连接无线网络](https://www.debugpoint.com/2020/11/connect-wifi-terminal-linux/)
ping -c 3 baidu.com

2.更新系统时间
timedatectl set-ntp true

3.硬盘分区
fdisk /dev/sda

4.分区格式化
mkfs.ext4 /dev/sda1

5.磁盘挂载
mount /dev/sda1  /mnt

6.置国内archlinux软件下载镜像源
vim /etc/pacman.d/mirrorlist
Server = https://mirrors.163.com/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

7.安装基本软件和kernel
pacstrap /mnt base base-devel linux linux-firmware

8.生成挂载信息文件
genfstab -U /mnt >> /mnt/etc/fstab

9.切换到新系统
arch-chroot /mnt

10.安装vim
pacman -S vim

11.设置时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc

12.设置本地化文本编码
vim /etc/locale.gen
zh_CN.UTF-8 UTF-8

locale-gen

vim /etc/locale.conf
LANG=en_US.UTF-8

13.设置主机名
vim /etc/hostname

14.配置hosts文件
vim /etc/hosts
127.0.0.1   localhost
::1         localhost
127.0.1.1   hellokitty.localdomain  hellokitty

15.管理员账号设置密码
passwd

16.创建新普通用户
useradd -m tom
passwd tom

17.安装grub,设置引导（bios主板）
pacman -S intel-ucode grub
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

17.2. 安装grub（UEFI主板）(查考UEFI系统：https://wiki.archlinux.org/index.php/GRUB)
pacman -S grub efibootmgr
fdisk -l （查看efi是在哪个分区）
mkdir /uefi;mount /dev/efi所在分区 /uefi
grub-install --target=x86_64-efi --efi-directory=/uefi --bootloader-id=自己命名

18.配置完毕
exit
reboot


```


## after first reboot, connect the wifi network
systemctl enable NetworkManager.service

```shell
#1.list nearby Wi-Fi networks
$ nmcli device wifi list
#2.connect to a wifi network
nmcli device wifi connect AP_NAME password PASSWORD
#3.show the connected wifi information
nmcli connection show
```
## change terminal font size
1. pacman -S terminus-font
2. configure
    - temporary changes
        setfont xxx(/usr/share/kbd/consolefonts/)
    - persistent change
        vim /etc/vconsole.conf
        ```shell
            FONT=ter-u32b
        ```


## configure the pacman mirrorlist
```shell
# /etc/pacman.d/mirrorlist
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.163.com/archlinux/$repo/os/$arch
```
pacman -Sy

## core software lists
|names |description |
|------|----------- |
|vim | |
|zsh | |
|git | |
|firefox | |
|python3 | |
|dropbear |a implementation of ssh/scp/.. |
|dropbear-scp |used to scp and ssh |




## git config
```shell
user.name=dere-arch
user.email=catt82401@gmail.com
core.editor=vim
```

## X server & i3-gaps
pacman -S xorg-xserver xorg-xinit
pacman -S i3-gaps compton feh i3status dmenu
NOTE:
|tools name |description |
|-----------|------------|
|compton |used to transparent the windows |
|feh |a tool to change the wallpaper |
|i3status |i3 buttom status line |
|dmenu |a application startup tool |


configure:
1. cp /etc/X11/xinit/xinitrc ~/.xinitrc
2. edit .xinitrc
```shell
#twm &
#xterm -geometry 80x50+494+51 &
#xterm -geometry 80x20+494-0 &
#exec xterm -geometry 80x66+0+0 -name login
exec feh --bg-max --randomize ~/wallpapers/* &
#exec fcitx &
exec i3
```
3. vim ~/.config/i3/config
```shell
set $mod Mod4
font pango:monospace 16
exec  --no-startup-id compton -b
```

## sound control

pacman -S alsa-utils
1. alsamixer -- a gui setting
    00 --> not mute
    MM --> is mute
    rotation key to switch items and set sound size
    
2. amixer sset Master unmute
 
## backlight control
1. if your grapic card is inter.
pacman -S xorg-xbacklight

2. otherwish, use a common interface controllor tools.
pacman -S acpilight

usage:
    xbacklight -get
    xbacklight -set 30
    xbacklight -inc 10
    xbacklight -dec 10

## shadowsocks-libev
1. download
pacman -S shadowsocks-libev asciidoc xmlto

```shell
git clone https://github.com/shadowsocks/simple-obfs.git
cd simple-obfs
git submodule update --init --recursive
./autogen.sh
./configure && make
make install
```

2. configure

- shadowsocks-libev(ss-local, ss-server...)
```shell
`/etc/shadowsocks/config.json`
{
    "server":"xxx",
    "server_port":"443",
    "local_ipv4_address":"127.0.0.1",
    "local_port":"1314",
    "password":"xxx",
    "method":"xchacha20-ietf-poly1305",

    "plugin":"obfs-local",
    "plugin_opts":"obfs=tls",

    "timeout": 10
}

`/usr/lib/systemd/system/shadowsocks-libev@.service`
ExecStart=/usr/bin/ss-local -c /etc/shadowsocks/%i.json -v

```

3. startup
systemctl start shadowsocks-libev@config.service
systemctl status shadowsocks-libev@config


## privoxy
> A tool convert socks5 proxy to http proxy.

1. download
pacman -S privoxy

2. configure
```shell
::::/etc/privoxy/config

//The privoxy work ip and port
listen-address 127.0.0.1:8118

//socks5 work ip & port
forward-socks5 / 127.0.0.1:1314 .

::::~/.zshrc
export http_proxy="http://localhost:8118"
```

3. start
systemctl start privoxy.service
systemctl enable privoxy.service



## aur (archlinux user repository)

### prebuild
> To run the makepkg can not be root, so first we should to add a aur user used to do this job.

```shell
useradd aur
passwd github
mkdir /home/aur && chown aur:aur /home/aur
visudo (Note: export VISUAL="vim")
    aur ALL=(ALL)ALL

### manual install aur package
1. download the PKGBUILD file (from archlinux offical website or git clone from the url)
2. makepkg -ris (r = remove[clean the instal], i = install[install the package], s = dependiency[auto download dependiency])



## systemd timer (used for Timers)
> Is build-in support for archlinux, need not to be install

### A demostration
1. create `.timer` suffix file in /etc/systemd/system/
```ini
#filename: foo.timer
[Unit]
Description=xxx

[Timer]
OnBootSec=10min
# OnCalendar format:
# DayOfWeek Y-M-D H-M-S
# 1. Mon,Tue *-*-01..04 12:00:00
# equal to under
# OnCalendar=*-*-*
OnCalendar=daily
# when the system boot from powered off that missed the last start time it triggers the sevice immediately
Persistent=true

[Install]
#systemd build-in target which sets up all timers that should be active after boot
WantedBy=timers.target
```

2. create `.service` suffix file in /etc/systemd/system/
```ini
#filename foo.service
[Unit]
Description=xxx

[Service]
Type=simple
ExecStart=/bin/bash /path/to/needed-to-exec.sh

```

3. startup
systemctl enable foo.timer
systemctl start foo.timer

### Note(relativly command)
systemctl status foo.service
systemctl list-timers






## archlinux虚拟机版本必要服务配置启动

```bash

1.网络配置
- 保证虚拟机网卡是桥接模式
- 查看是否存在无线网卡
ls /sys/class/net/en*/
如果存在wireless或phy80211文件夹，存在无线网卡
- 使用systemd-networkd来管理网络
2.为有线网卡配置静态ip
vim /etc/systemd/network/20-wired.network

[Match]
Name=ensxx
[Network]
Address=192.168.1.200/24
Gateway=192.168.1.1
DNS=192.168.1.1
DNS=8.8.8.8
DNS=114.114.114.114

3.设置后台服务（systemd-networkd.service）开机启动
systemctl enable systemd-networkd.service


4.安装sshd
pacman -S openssh
systemctl start sshd
systemctl enable sshd.service

5.让普通用户使用sudo权力
sudo -
chattr -i /etc/sudoers
vim /etc/sudoers
xxx ALL=(ALL)ALL
chattr +i /etc/sudoers

```









