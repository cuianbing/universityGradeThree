---
title: Centos7 搭建Hadoop环境 - 1配置静态IPV4和禁止IPV6
tags: 
grammar_cjkRuby: true
---


## 查看IP地址
``` shell?linenums
ifconfig
```


有可能会出现出-bassh ofconfig :command not found(找不到这个命令)。

![ifconfig报错信息][1]

该命令已经过时了，而且在最小化版本的RHEL 7以及它的克隆版本CentOS 7，Oracle Linux 7和Scientific Linux 7中也找不到该命令。
CentOS 7最小化系统，使用“ip addr”和“ip link”命令来查找网卡详情。要知道统计数据，可以使用“ip -s link”。
要查看网卡细节，输入以下命令：
``` shell?linenums
ip addr
```
![查看IP信息][2]
如果你想继续使用ipconfig命令，可在联网的情况下安装net-tool软件
``` shell?linenums
yum install net-tools
```
## 修改IP地址

1. 进入到网卡挂载目录下,并且查看网卡信息
``` shell?linenums
cd /etc/sysconfig/network-scripts/
ls
```
![网卡挂载目录][3]

这里需要使用的配置文件是:ifcfg-enp2s5,每个人的可能不一样,切换root权限,通过vim进到里面,可以看到里面的内容:

![网卡配置文件][4]

2. 这里说一下需要修改的位置:
> #修改
BOOTPROTO=static #这里讲dhcp换成ststic
ONBOOT=yes #将no换成yes
#新增
IPADDR=192.168.1.102 #静态IP
GATEWAY=192.168.1.1 #默认网关
NETMASK=255.255.255.0 #子网掩码

保存退出后,重启网络服务:
``` shell?linenums
service network restart
Restarting network (via systemctl):                        [  确定  ]
```
查看当前ip:

``` shell?linenums
ip addr
```

![查看ip地址][5]


  [1]: ./images/1536225877291.jpg
  [2]: ./images/1536225896680.jpg
  [3]: ./images/1536226114016.jpg
  [4]: ./images/1536226191065.jpg
  [5]: ./images/1536226989216.jpg