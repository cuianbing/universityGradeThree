---
title: Centos7 搭建Hadoop环境 - 03配置无密登录到所有节点上
tags: 
grammar_cjkRuby: true
---

## 生成公钥和私钥
1. 使用命令 ssh-keygen -t rsa  生成秘钥，当有提示输入时，直接按回车使用默认值
``` shell?linenums
ssh-keygen -t rsa
```

![生成秘钥][1]

使用将公钥追加到各台机器上
``` shell?linenums
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.1.100
```
命令格式为 ssh-copy-id -i  本机公钥文件  远端用户名@远端地址
或者使用命令
``` shell?linenums
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
scp ~/.ssh/authorized_keys root@192.168.1.100:~/.ssh/
```
先将公钥追加到authorized_keys文件中，然后authorized_keys文件发送到其他机器上。

 [1]: ./images/1536234957129.jpg