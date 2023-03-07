---
title: centos7
date: 2021-07-21 18:01:12
categories:
- linux
tags:
- centos
---





### VMware中的3种网络模式

![](/images/net模式.png)

1. 桥接模式：相当于虚拟机直接连接路由器
2. 仅主机模式：只能所在主机单向访问虚拟机，无法连接外网
3. nat模式：主机和虚拟机之间可以双向通讯，且通过nat网络映射的原理能实现连接外部网络。但局域网中的其他主机一般无法访问此虚拟机的服务。

### Linux防火墙和yum+epel

1. 查看Linux内核版本
cat /proc/version    或者 uname -a   

2. 查看linux发行版（什么系统centos？Fedora？，版本号）
  lsb_release -a  , cat /etc/issue  适用于所有发行版Redhat，suse，debian
  cat /etc/redhat-release   适用于Redhat系列的linux
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

3. centos的第7个版本，（也就是通过cat /etc/redhat-release 查看的有centos7字样的。）中的防火墙不再是iptables，而是firewalld。

firewalld是一个后台进程，分为系统配置文件和用户配置文件，系统配置文件（/usr/lib/firewalld/services），用户配置文件（/etc/firewalld/）

修改防火墙的设置有两种方法。
  1.直接修改用户配置文件，
  2.使用命令（firewall-cmd）来间接管理修改配置文件

  firewall-cmd --permanent --add-port=445/tcp 开放9527端口
  firewall-cmd --permanent --remove-port=12345/tcp  禁用端口
  firewall-cmd --state        查看防火墙的状态
  firewall-cmd --list-all       查看防火墙的开放列表


firewalld 常用命令
1.重启，关闭，开启firewalld服务
service firewalld start/stop/restart


4. 开放80端口(centos6)
            vi /etc/sysconfig/iptables
            -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
            service iptables restart
            或:
            iptables -A INPUT -p tcp -m state --state  NEW  -m tcp --dport 80 -j ACCEPT
            service iptables save
            service iptables restart 




（https://blog.csdn.net/yyycheng/article/details/79753032）
（https://blog.csdn.net/xiazichenxi/article/details/80169927）防火墙详细介绍
（https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/7/html/security_guide/sec-using_firewalls）redhat官方文档
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


### centos系统安装流程

1. 安装centos7，通过镜像

2. 配置网络
    vi /etc/sysconfig/network-scripts/ifcfg-xxx
          {
            修改如下:
            static
            on
            添加如下:
            IPADDR=192.168.1.100
            NETMASK=255.255.255.0
            GATEWAY=192.168.1.1
            DNS1=114.114.114.114
            DNS2=8.8.8.8
            DNS3=192.168.1.1(/etc/resolve.conf文件会根据网卡配置文件中的dns值设置nameserver，因此会出现每次重启导致resolve.conf文件清空)
          }
        systemctl restart network.service(service network restart)
        ip addr
    
3. 配置yum国内镜像源
    
    3.1 首先备份/etc/yum.repos.d/CentOS-Base.repo
          mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
    
     3.2 在163镜像站里找到centos对应版本repo文件下载
          curl http://mirrors.163.com/.help/CentOS7-Base-163.repo > /etc/yum.repos.d/CentOS7-Base-163.repo
    
    3.3 生成缓存
          yum clean all
          yum makecache
    
4. 安装常用软件
    yum install gcc vim git wget

5. bashrc配置

    ```shell

    # .bashrc

    # User specific aliases and functions

    alias rm='rm -i'
    alias cp='cp -i'
    alias mv='mv -i'

    # Source global definitions
    if [ -f /etc/bashrc ]; then
      . /etc/bashrc
    fi


    alias cl='cl(){ cd $1;ls;};cl'
    alias scpget='scpget(){ scp root@192.168.1.101:$1 $2;};scpget'
    alias scpsend='scpsend(){ scp root@192.168.1.101:$1 $2;};scpsend'
    alias home='home(){ cl ~;};home'
    alias portadd='portadd() { firewall-cmd --permanent --add-port=$1/tcp;};portadd'
    alias portsub='portsub(){ firewall-cmd --permanent --remove-port=$1/tcp;};portsub'
    alias net='net(){ netstat -lnp |grep $1;};net'

    ```

6. 安装zsh和下载zsh的配置文件oh-my-zsh

+++ 安装zsh +++
yum install -y zsh

+++ 安装oh-my-zsh +++
git clone https://gitee.com/mirrors/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

7. 配置网络时间同步服务ntpd
https://zhuanlan.zhihu.com/p/156757418


<br>
<br>

### centos软件管理
> References:
> https://blog.csdn.net/liang_operations/article/details/83241551#2rpm_7

1. 软件类型
  rpm--》二进制软件。
  tar--》软件源码。

2. rpm管理（1.手动下载安装。 2.使用yum包管理软件来管理）
    - 手动管理
        1. rpm包格式
        eg：perl-Carp-1.26-244.el7.noarch.rpm
        软件名：    perl-Carp
        版本：     1.26-244  （版本细分：主版本号1，次版本号26，release次数244）
        操作系统版本： e17
        cpu架构：    noarch
      
        2. 下载
            通过互联网下载（curl或wget）xxx.rpm
        3. 安装
            rpm -ivh xxx.rpm
        4. 查询
            rpm -qa xxx （查询是否安装某软件）
            rpm -qf /usr/bin/dd （查询软件所属rpm包）
        5. 删除
            rpm -e xxx.rpm

    - yum软件来管理
        1. yum原理
        首先yum软件运行makecache选项后通过目录/etc/yum.repos.d/下的配置文件，
        通过网络更新软件信息缓存表（只是软件的描述信息，而不是软件本身）
        用户就可以查询相关软件的信息在本地，通过缓存表
        yum clean all 清空缓存表（/var/cache/yum）
        yum makecache 生成缓存
        
        ```bash
        1、升级系统
        [root@localhost ~]#yum update
         
        2、安装指定的软件包
        [root@localhost ~]# yum -y install mysql-server
         
        3、升级指定的软件包
        [root@localhost ~]# yum -y update mysql
         
        4、卸载指定的软件包
        [root@localhost ~]# yum -y remore mysql
         
        5、查看系统中已经安装的和可用的软件组，对于可用的软件组，你可以选择安装
        [root@localhost ~]# yum grouplist
         
        6、安装上一个命令中显示的可用的软件组中的一个软件组
        [root@localhost ~]# yum -y groupinstall Emacs
         
        7、更新指定软件组中的软件包
        [root@localhost ~]# yum -y groupupdate Emacs
         
        8、卸载指定软件组中的软件包
        [root@localhost ~]# yum -y groupremove Emacs
         
        9、清除缓存中的rpm 头文件和包文件
        [root@localhost ~]# yum clean all
         
        10、搜索相关的软件包
        [root@localhost ~]# yum -y search Emacs
         
        11、显示指定软件包的信息
        [root@localhost ~]# yum info Emacs
         
        12、查询指定软件包的依赖包
        [root@localhost ~]# yum deplist emacs
         
        13、列出所有以 yum 开头的软件包
        [root@localhost ~]# yum list yum\*
         
        14、列出已经安装的但是不包含在资源库中的rpm 包
        [root@localhost ~]# # yum list extras
        ```

3. tar源码包管理
    ./configure --prefix = /usr/local/bin/    （配置软件的属性：安装路径，功能选项。类似win下单安装软件过程。最终生成makefile文件）
    make      根据makefile规则执行编译
    make install  根据makefile规则执行安装
    make clean    根据makefile规则执行清理


4. 自己安装软件,设置环境变量
    1. 建立可执行文件的软连接.
    ln -s src dst
    2. vim .bashrc
    export PATH=$PATH:<yourpath>
    3. source .bashrc


<br>
<br>


### centos7搭建web服务器

#### nginx + php方式
1. 安装nginx
方式一：
    yum install epel-release
    yum makecache
    yum install nginx -y
方式二：
        cat /etc/redhat-release 发行版本号,下面的6
            uname -a //内核版本x.x.x
            vi  /etc/yum.repos.d/nginx.repo

            [nginx]
            name=nginx repo
            baseurl=http://nginx.org/packages/centos/6/$basearch/
            gpgcheck=0
            enabled=1
        
            yum clean all
            yum makecache
            yum install nginx
    
2. 安装php
    rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm   ==添加源
    yum search php7x   ==查找包
    yum install php7x*   ==安装7x的所有包(扩展)

3. 配置nginx
    1.修改
        location / {
            root   E:\php-7.2.6\www;   //php目录
            index  index.html index.htm;
        }
  2.解开如下location节点
        location ~ \.php$ {
            root           E:\php-7.2.6\www;   //php目录
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name; //修改/script为$document_root
            include        fastcgi_params;
        }

4. 配置php
    3.配置
      3.1 解开配置
      extension_dir = E:\php-7.2.6\ext;//php插件目录
      cgi.fix_pathinfo=1; //php与使用cgi的配置项
    4.管理
      4.1.启动
          php-cgi.exe -b 127.0.0.1:9000 -c php.ini
      4.2.关闭

<br>
<br>

#### tomcat + java方式
1. 首先下载java
  yum clean all
  yum makecache 
  yum install java
2. 下载tomcat安装包
  国内超快镜像  https://mirrors.cnnic.cn/apache/tomcat/

3. 解压tar.gz
  tar -zvxf apache-tomcat.tar.gz
  cd apache-tomcat/bin
  ./starup.sh

4. 查看tomcat实时日志记录
  cd apache-tomcat/logs
  tail -f catalina.out
  ctrl+c退出

<br>
<br>

#### 数据库下载
> centos7中不再使用mysql，MySQL被收购之后就不是开源了。使用的是mariadb数据库

1. 安装mariadb
yum install mariadb-server mariadb
2. 启动mariadb
systemctl start mariadb
3. 关闭mariadb
systemctl stop mariadb

#### 服务管理
1.查看启动项
  systemctl list-unit-files
2.过滤查看启动项 
  systemctl list-unit-files | grep enable 


