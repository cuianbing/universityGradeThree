---
title: Hadoop环境搭建   
tags: 2018-9-3 Hadoop
grammar_cjkRuby: true
---
# Hadoop环境搭建手册
# 环境概述
- 阿里云
- 操作系统 Centos  6.8
- Java版本  jdk  1.8
- SSH

# 目录
1. 安装操作系统
2. 网络环境配置
3. 配置SSH无密登录
4.  安装并配置Java环境
5.  下载安装Hadoop
6. 配置Hadoop
7. 启动Hadoop并测试
8. 单词统计示例

# 安装操作系统
在阿里云上安装操作是一个很简便的操作。操作系统的选择Centos6.8 x64（64位）。
有些同学不具有多台阿里云的机器，也可以在VM上安装虚拟机，进行操作，安装虚拟机的教程请参考......

# 网络环境配置
## 查看机器IP地址设计网络结构
1. 查看阿里云的IP地址，用的是公网地址
2. ![阿里云界面][1]
在这里IP地址采用虚拟的。
IP地址与及其对应

|   IP地址  |   主机名  |   功能  |   备注  |
| --- | --- | --- | --- |
|   192.168.1.101  |  master   |  NameNode   |     |
|   192.168.1.102  |  slave1   |   DataNode     |     |
|   192.168.1.103  |  slave2   |   DataNode     |     |

网络拓扑图：
![网络架构图][2]
## 配置主机名
1. 临时修改主机名

- 显示主机名：
``` shell
[root@localhost ~]# hostname
master
```
- 修改主机名：
``` shell
[root@localhost ~]# sudo hostname hadoop
[root@localhost ~]# hostname
hadoop
 ```
PS:以上的修改只是临时修改，重启后就恢复原样了。

2. 永久修改主机名
redhat/centos上永久修改
``` shell
[root@localhost ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=localhost.localdomain
GATEWAY=192.168.10.1
```
修改network的HOSTNAME项。点前面是主机名，点后面是域名。没有点就是主机名。
``` shell
[root@localhost ~]# vim /etc/sysconfig/network
NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=master
```
这个是永久修改，重启后生效。或者使用命令
``` shell
 [root@localhost ~]#  source /etc/sysconfig/network
```
使用命令后重新打开终端（控制台）
想立即生效，可以同时采用第一种方法。
还有一个修改是：
/etc/hosts文件
在下面这一行最后面添加你修改后的主机名
``` shell
[root@localhost ~]#  vim /etc/hosts
```
> 127.0.0.1   localhost.localdomain
> 修改为 127.0.0.1   localhost.localdomain  master

修改主机名要在所有的机器上进行

- 关闭防火墙
学习环境可以直接把防火墙关闭掉。
(1) 用root用户登录后，执行查看防火墙状态
```shell
[root@master ~]# service iptables status
```
(2) 用[root@master~]# service iptables stop关闭防火墙，这个是临时关闭防火墙。
``` shell
[root@master ~]# service iptables stop
iptables: Setting chains to policy ACCEPT: filter  [  OK  ]
iptables: Flushing firewall rules:            [  OK  ]
iptables: Unloading modules:                [  OK  ]
```
(3) 如果要永久关闭防火墙用。
``` shell
[root@master ~]# chkconfig iptables off
```
## 配置hosts文件
``` shell
[root@master ~]# vim /etc/hosts
```
添加以下内容
> 192.168.1.101   master
> 192.168.1.102   slave1
> 192.168.1.103   slave2

添加后保存并退出，使其立即生效
``` shell
[root@master ~]# source  /etc/hosts
```
重新打开终端（控制台），使用命令检查配置是否成功
``` shell
[root@master ~]# ping slave1
```
![成功显示结果][3]

按ctrl+c结束命令。同样测试slave2机器
``` shell
[root@master ~]# ping slave2
```
# 配置多台机器的SSH无密码登录
## 配置ssh，实现无密码登陆
无密码登陆，效果也就是在master上，通过 ssh slave1 或 ssh slave2  就可以登陆到对方计算机上。而且不用输入密码。
三台虚拟机上，使用命令  ssh-keygen -t rsa    一路按回车就行了。
刚才都作甚了呢？主要是设置ssh的密钥和密钥的存放路径。 路径为~/.ssh下。
打开~/.ssh 下面有三个文件
``` shell
[root@master ~]# cd ~/.ssh
```
>authorized_keys，已认证的keys
id_rsa，私钥
id_rsa.pub，公钥   三个文件。

下面就是关键的地方了，（我们要做ssh认证。进行下面操作前，可以先搜关于认证和加密区别以及各自的过程。）
①在master上将公钥放到authorized_keys里。使用命令
``` shell
[root@master ~]# sudo cat id_rsa.pub >> authorized_keys
```

 ②将master上的authorized_keys放到其他linux的~/.ssh目录下。这里会提示输入密码。
 ``` shell
[root@master ~]# sudo scp authorized_keys root@slave1:~/.ssh
[root@master ~]# sudo scp authorized_keys root@slave2:~/.ssh
```
> 命令格式为 sudo scp authorized_keys 远程主机用户名@远程主机名或ip:存放路径。

③修改authorized_keys
```shell
chmod 644 ~/.ssh/authorized_keys
```
④测试是否成功
       ssh slave1 输入用户名密码，然后退出，再次ssh slave2不用密码，直接进入系统。这就表示成功了

# 安装并配置Java环境




# 下载安装Hadoop





# 配置Hadoop环境





# 启动Hadoop并测试




# 单词统计示例


  [1]: ./images/1535935139696.jpg
  [2]: ./images/1535937042269.jpg
  [3]: ./images/1535939091331.jpg