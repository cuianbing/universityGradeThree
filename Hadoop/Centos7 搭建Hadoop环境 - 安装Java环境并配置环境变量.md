---
title: Centos7 搭建Hadoop环境 - 安装Java环境并配置环境变量
tags: 
grammar_cjkRuby: true
---

## 安装Java环境
### 检查和卸载原有的jdk
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

### 安装下载好的jdk
#### 解压文件并重命名
1. 将下载好的文件上传到机器上，[下载地址][5]，在这里选择对应的版本。
2. 解压文件，使用tar将文件解压到指定目录下
``` shell?linenums
tar -zxvf jdk-8u181-linux-x64.tar.gz  -C /software/java/
```
命令格式为 tar -zxvf 文件名  -C 指定路径
解压后看到指定目录下多出一个文件夹，将文件夹重命名
``` shell?linenums
mv jdk1.8.0_181/ jdk1.8/
```
命令格式为 mv 原文件夹名/  新文件夹名/
#### 配置环境变量

使用VIM修改/etc/profile配置文件
``` shell?linenums
vim /etc/profile
```
![/etc/profile配置文件][6]
进入后使用方向键到文件末尾，按字母o进入插入模式。（在VIM中不能使用鼠标）
添加以下文本：
> JAVA_HOME=/software/java/jdk1.8
CLASSPATH=.:\$JAVA_HOME/lib/dt.jar:\$JAVA_HOME/lib/tools.jar
PATH=\$PATH:\$JAVA_HOME/bin
export JAVA_HOME CLASSPATH PATH

![修改后的配置文件][7]
使用命令使配置文件立即生效（也可以直接重新启动）
``` shell?linenums
source /etc/profile
```
使用 java -version 查看是否配置成功
``` shell?linenums
java -version
```
![查看Java安装是否成功][8]


  [1]: ./images/1536222039255.jpg
  [2]: ./images/1536222266579.jpg
  [3]: ./images/1536222487086.jpg
  [4]: ./images/1536222574189.jpg
  [5]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
  [6]: ./images/1536223181228.jpg
  [7]: ./images/1536223501484.jpg
  [8]: ./images/1536223892492.jpg