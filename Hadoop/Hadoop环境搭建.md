---
title: Hadoop环境搭建   
tags: 2018-9-3 Hadoop
grammar_cjkRuby: true
---

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
5.  配置SSH无密登录
6. 

# 安装操作系统
在阿里云上安装操作是一个很简便的操作。操作系统的选择Centos6.8 x64（64位）。
有些同学不具有多台阿里云的机器，也可以在VM上安装虚拟机，进行操作，安装虚拟机的教程请参考......

# 网络环境配置
## 
1. 查看阿里云的IP地址，用的是公网地址
2. ![阿里云界面][1]
在这里IP地址采用虚拟的。
IP地址与及其对应

|   IP地址  |   主机名  |   功能  |   备注  |
| --- | --- | --- | --- |
|   192.168.1.101  |  master   |  NameNode   |     |
|   192.168.1.102  |  slave1   |   DataNode     |     |
|   192.168.1.103  |  slave2   |   DataNode     |     |

![网络架构图][2]
## 配置主机名
1. 临时修改主机名

- 显示主机名：
spark@master:~$ hostname
master
- 修改主机名：
spark@master:~$ sudo hostname hadoop
spark@master:~$ hostname
hadoop
 
PS:以上的修改只是临时修改，重启后就恢复原样了。

2. 永久修改主机名
redhat/centos上永久修改
[root@localhost ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=localhost.localdomain
GATEWAY=192.168.10.1
修改network的HOSTNAME项。点前面是主机名，点后面是域名。没有点就是主机名。
[root@localhost ~]# vi /etc/sysconfig/network
NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=master
这个是永久修改，重启后生效。目前不知道怎么立即生效。
想立即生效，可以同时采用第一种方法。
还有一个修改是：
/etc/hosts
在下面这一行最后面添加你修改后的主机名
> 127.0.0.1   localhost.localdomain
> 修改为 127.0.0.1   localhost.localdomain  master

修改主机名要在所有的机器上进行


# 配置多台机器的SSH无密码登录


  [1]: ./images/1535935139696.jpg
  [2]: ./images/1535937042269.jpg