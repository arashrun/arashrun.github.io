---
title: Linux知识大全
date: 2021-07-12 12:23:33
categories:
- linux
tags:
- linux
---


> 记录范围：
>   1.关于Linux方面的通用概念
>   2.linux上好用的必备软件




### bash终端代理设置

1. windows下的ssr的选项设置下的本地代理勾选中
2. [Linux bash终端设置代理（proxy）访问](http://aiezu.com/article/linux_bash_set_proxy.html#:~:text=linux%E8%A6%81%E5%9C%A8s,%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E5%8D%B3%E5%8F%AF%E3%80%82)
```bash
#.bashrc
export http_proxy=10.0.0.52:1080
export https_proxy=10.0.0.52:1080
export no_proxy="*.aiezu.com,10.*.*.*,192.168.*.*,*.local,localhost,127.0.0.1"

```

`proxyman` : 一键配置代理脚本



### 终端翻译软件(shell translation tools)
sudo apt-get install ruby
sudo gem install fy




## ssh及数据加密相关



> s sh（secure shell）是一种协议，用于应用层协议下，常用于对应用数据进行加密。是一种数据加密的协议。常用的sshd服务（linux）和openssh（win）就是基于此协议的应用（用于安全的远程登录，以前使用telnet进行远程登录）

----------------------------------------------------------------------------------

### 通过修改服务器的ssh服务配置信息 ，实现ssh客户端（要连接的主机）的免密登录

  1.1 首先在客户端使用openssh(windows上的)的ssh-keygen生成公私钥（生成位置：~/.ssh/）
    ssh-keygen -t rsa
  1.2 将公钥通过scp发送给服务端，并保存到（/root/.ssh/authorized_keys） 文件中
    scp id_rsa.pub root@ip地址:文件保存路径
    cat id_rsa.pub >> /root/.ssh/authorized_keys
    chmod 600 /root/.ssh/authorized_keys（为了安全修改文件权限）
  1.3修改服务器中的sshd的配置文件（sshd_config），修改如下几项，没有自行添加
    RSAAuthentication yes   (开启rsa验证)
    PubkeyAuthentication yes  (是否使用公钥验证)
    AuthorizedKeysFile  .ssh/authorized_keys (公钥的保存位置)
  1.4 重启ssh服务
    service sshd restart

【注】 。ssh属于数据加密中的非对称加密(有公钥和私钥，对称机密秘钥都是相同的)
  。ssh-keygen 的选项有（-t 使用的加密算法类型 -p 保护私钥的安全 -f 指定秘钥存放的文件，带pub后缀的为公钥）
  。https://www.cnblogs.com/olio1993/p/10960306.html ssh加密的理解


### a免密登b

    1. 在a中生成公私密钥
        ssh-keygen -t rsa -P ""
    2. 将公钥(.pub)发送到b中
    3. cat xxx.pub >> authorized_keys


### ssh的高级配置优化连接（具体就不写了，因为没看，需要时再查看）

  。https://blog.csdn.net/boshuzhang/article/details/69524800  ssh的配置文件
  。https://www.douban.com/note/666554273/   客户端配置长连接和共享连接
  。https://blog.csdn.net/chenqijing2/article/details/79098703/  ssh详细参数介绍










## squid代理服务器搭建

yum install squid 
vi /etc/squid/squid.conf
//检查配置文件
squid -k parse
squid start
//关闭服务
squid -k shutdown


shadowsocks 基本操作
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh  安装
chmod +x shadowsocks-libev.sh
./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log   运行
./shadowsocks-libev.sh uninstall  xie zai
/etc/init.d/shadowsocks start qidong
/etc/init.d/shadowsocks stop  ting zhi 

> References:
> [squid基本的配置和运行管理命令](https://www.cnblogs.com/bluestorm/p/9032086.html)
> [squid+ssl+stunnel，建立加密信道连接实现代理越墙](https://www.hawu.me/operation/886)
> [shadowsocks详细操作](https://www.williamlong.info/archives/4680.html)





## iptables

### table

### chains

### rules

#### matches

#### target






## linux磁盘管理及文件系统

### lvm及磁盘扩容
pv = physic volume(物理卷)
vg = volume group(卷组)
lv = logic volume(逻辑卷)

-----

```shell
- VMware将磁盘扩容，不是添加sdb，sdc
- 磁盘分区
- 将分区转化为pv
- 将pv加入某一vg
- 将vg进行extend操作
- 从vg种划分出多个pe出来成为lv
```

### linux下查看磁盘分区的文件系统格式
1.df -T 只可以查看已经挂载的分区和文件系统类型
2.fdisk -l 可以显示出所有挂载和未挂载的分区，但不显示文件系统类型
3.parted -l 可以查看未挂载的文件系统类型，以及哪些分区尚未格式化
4.lsblk -f 也可以查看未挂载的文件系统类型
5.file -s /dev/sda3

### 磁盘分区管理

1. 分区：表示对硬盘进行切割的操作
2. 分区类型：mbr gpt 【形象地理解就是对硬盘切割的方式不同，可能一个横着切，一个竖着切。且不同分区类型支持的硬盘大小也不同】 
3. 分区标记：root swap boot bios-grub hidden lvm 【这些都是对被分区盘块的标识，如同一个衣柜，根据每个小隔间起一个名字，这块叫上衣间，那块叫裤子间，那块叫袜子间。】
4. 挂载点：/ /home /boot /usr 【分完区，贴上标记之后。我们为了能够访问不是root分区的盘块，必须将其他分区通过一根线(挂载线)，和root分区挂钩。因为linux只能逻辑上访问root分区中的数据，我们通过挂载点来访问非root分区数据】
5. 文件系统：ext3 ext4 ntfs 【分完区之后，我们还要细分割每个分区之后才能使用。文件系统代表分区的分割方式】

## linux的grub相关知识
[通过grub-install命令把grub安装到u盘-总结](https://blog.csdn.net/mao0514/article/details/51218522)
[GRUB Rescue 恢复](https://www.cnblogs.com/Dumblidor/p/6056948.html)
[GRUB 原理及安装+grub手动引导启动](http://www.cnblogs.com/yinheyi/p/7279508.html)
Linux中grub写入U盘
vboxmanage internalcommands createrawvmdk -filename d:\\usb23.vmdk -rawdisk \\.\PhysicalDrive1 -partitions 2,3,4













## bash命令与脚本

<h3 style="color:red;">bash命令</h3>

#### 图形界面和命令行界面切换
图形界面下---ctrl+alt+f[1-6]
命令行下----ctrl+alt+f7

#### locate,whereis,which,find

> [locate参考链接](https://www.cnblogs.com/xqzt/p/5426666.html)
>
> [四者的区别](https://www.cnblogs.com/jycjy/p/6940544.html)

- find：实时查找，精确查找，但速度慢。

- locate:命令不是实时查找，所以查找的结果不精确，但查找速度很快。因为它查找的不是目录，而是一个数据库（/var/lib/locatedb），这个数据库中含有本地所有文件信息Linux系统自动创建这个数据库，并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。为了避免这种情况，可以在使用locate之前，先使用updatedb命令，手动更新数据库。

- which:查找命令类型的文件

- whereis：只搜索，二进制文件，man手册，源文件



#### history命令

> 该命令读取家目录下的.bash_history文件。
>
> 参看链接： https://www.cnblogs.com/wxxjianchi/p/9588916.html

```bash
history [n]
-c： 将目前shell中的所有history命令消除。对命令历史文件没有影响
-w ：将本次登录的命令写入命令历史文件中
-r ： 将命令历史文件中的内容读入到目前shell的history记忆中

! number 执行第几条命令  
! command 从最近的命令查到以command开头的命令执行
!! 执行上一条
```



#### grep命令



```shell
grep -r "内容" dir
返回:文件名+内容

grep -r -l '内容' dir
返回:文件名

-n 显示行号
-i 忽略大小写
-r 递归搜索子目录(recursive)

```



#### find命令

```shell
find . -name "*.c"
在当前目录及子目录下找出所有.c文件

-type c 类型为c的文件(c= d目录,f一般文件, l符号链接, )
-maxdepth 1 非递归查询,默认递归

```



#### 多用户管理

```bash
1.查看在线用户
w -s
2.查看某用户运行的进程
ps
3.获得某个终端下（tty）所有进程
ps -t /dev/pts/xxx
4.优雅退出进程
kill pid （向进程发送sigterm信号）
kill -9 pid （发送sigkill，不等进程处理）
```



---
<h3 style="color:red;">bash脚本</h3>

#### alias传参

```shell
alias不支持传递命令行参数，所以需要自定义函数来实现外部参数的使用

alias scpget='scpget(){ scp root@ip:$1 $2;};scpget'    #注意最后的两个分号和{ 右边的空格
alias net='net(){ netstat -lnp |grep $1;};net'          #查看指定端口占用情况
```

#### bash数组
```shell
##数组定义
nums=(29 100 13 8 91 44)
arr=(20 56 "http://c.biancheng.net/shell/")
ages=([3]=24 [5]=19 [10]=12)
##获取数组元素
${array_name[index]}
使用@或*可以获取数组中的所有元素，例如：
${nums[*]}
${nums[@]}

```

#### date命令
```shell
date +'%c'	#输出日期和时间
```

#### inotifywait命令监控文件变化
> 1. [inotifywait命令参数](https://www.cnblogs.com/zhanbing/p/10976796.html)
> 2. [检测文件变化](https://blog.csdn.net/beeworkshop/article/details/111349815?ops_request_misc=&request_id=&biz_id=102&utm_term=shell%E7%9B%91%E6%8E%A7%E6%96%87%E4%BB%B6%E5%A4%B9%E5%8F%98%E5%8C%96&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-111349815.pc_search_result_no_baidu_js)

eg:
监测root，etc及递归下的文件变化
inotifywait -mrq --format '%e' --event create,delete,modify  /root/ /etc/

#### bash脚本中，将内容完整输出到文件
```bash
cat <<EOF > /home/xxx
fadfasdfas
dfasdfadf
fasdfasd
fadfasdfasfadfasdfa
EOF
```

