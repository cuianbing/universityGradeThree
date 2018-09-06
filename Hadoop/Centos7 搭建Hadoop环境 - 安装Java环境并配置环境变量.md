---
title: Centos7 搭建Hadoop环境 - 安装Java环境并配置环境变量
tags: 
grammar_cjkRuby: true
---

## 安装Java环境
1. 查看是否有自带的Java环境（一般为OpenJdk）
``` shell?linenums
java -version
```
![返回结果][1]
2. 如果存在，卸载之前的环境（如果不存在忽略这一步）
使用命令查看Java文件名：
``` shell?linenums
rpm -qa | grep java
```
![返回结果][2]
然后通过    rpm -e --nodeps   后面跟系统自带的jdk名    这个命令来删除系统自带的jdk，
``` shell?linenums
rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.161-2.b14.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.171-2.6.13.2.el7.x86_64
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.161-2.b14.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.171-2.6.13.2.el7.x86_64
```
![卸载openjdk][3]
检查是否卸载成功

![成功][4]


  [1]: ./images/1536222039255.jpg
  [2]: ./images/1536222266579.jpg
  [3]: ./images/1536222487086.jpg
  [4]: ./images/1536222574189.jpg