---
title: CentOS7系统卸载自带的OpenJDK并安装SUNJDK
date: 2017-01-16 21:37:16
categories: Linux
tags: [Linux,CentOS7,Java,jdk]
---
<Excert in index | 首页摘要>
CentOS7卸载OpenJDK并安装SunJDK
<!-- more -->
<The rest of contents | 余下全文>
## 安装说明
- 系统环境： CentOS 7
- 安装方式： rmp安装
- 软件： jdk-8u111-linux-x64.rpm
- 下载地址： [Oracle JDK 官网下载地址](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
## 卸载CentOS默认安装的OpenJDK
### 检查CentOS7默认安装版本
```
[root@localhost ~]# java -version
java version "1.7.0_45"
OpenJDK Runtime Environment (rhel-2.4.3.3.el6-x86_64 u45-b15)
OpenJDK 64-Bit Server VM (build 24.45-b08, mixed mode)
```
### 查看JDK安装的rpm信息
```
[root@localhost ~]# rpm -qa |  grep java
java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.x86_64
tzdata-java-2013g-1.el6.noarch
java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.x86_64
```
### 卸载找出的已安装Java相关rpm文件
```
[root@localhost ~]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.x86_64
[root@localhost ~]# rpm -e --nodeps tzdata-java-2013g-1.el6.noarch
[root@localhost ~]# rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.x86_64
```
## 安装SunJDK
### 下载SunJDK的rpm安装文件
例如：我放到目录/usr/local/，然后使用rpm命令安装
```
[root@localhost ~]# rpm -ivh jdk-8u111-linux-x64.rpm
```
jdk默认安装路径为：/usr/java/
### 验证是否正确安装
```
[root@localhost ~]# java -version
java version "1.8.0_111"
Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)
```
## 配置环境变量
### 打开环境变量存储文件
```
[root@localhost ~]# vi /etc/profile
```
### 在文件末尾追加以下内容
```
# SunJDK 1.8
JAVA_HOME=/usr/java/jdk1.8.0_111
JRE_HOME=/usr/java/jdk1.8.0_111/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
```
### 使修改的环境变量起作用
```
[root@localhost ~]# source /etc/profile 
```
### 检查系统环境变量PATH的值
```
[root@localhost ~]# echo $PATH
/usr/local/cmake/bin:/usr/lib64/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/java/jdk1.8.0_111/bin:/usr/java/jdk1.8.0_111/jre/bin:/root/bin
```
