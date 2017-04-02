---
title: centos7查看系统信息命令
date: 2017-03-15 16:27:36
categories: linux
tags: [Linux,CentOS7,系统信息]
---
<Excert in index | 首页摘要>
CentOS7查看系统信息
<!-- more -->
<the rest of contents | 余下全文>

> 以下命令运载CentOS7中测试通过

## Linux查看服务器系统信息

### CentOS版本

```
[root@blog ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

```

### LSB - Linux Standard Base
** 需要安装redhat-lsb **
```
[root@localhost ~]# yum install redhat-lsb

```

使用lsb命令查看

```
[root@localhost ~]# lsb_release -a
LSB Version:	:core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID:	CentOS
Description:	CentOS Linux release 7.1.1503 (Core) 
Release:	7.1.1503
Codename:	Core
```

### /etc/redhat-release文件查看

```
[root@blog ~]# cat /etc/redhat-release 
CentOS Linux release 7.0.1406 (Core)
```

### 查看主机名/内核版本/CPU架构

```
[root@blog ~]# uname -n -r -p -o
blog.inspur.com 3.10.0-123.el7.x86_64 x86_64 GNU/Linux

```

### 查看Linux系统字符集设置

```
[root@blog ~]# echo $LANG $LANGUAGE
zh_CN.UTF-8
```

## 查看用户

### 查看当前登录用户

```
[root@blog ~]# whoami
root
```

### 查看当前用户及其所属组

```
[root@blog ~]# id
uid=0(root) gid=0(root) 组=0(root)

```

### 查看当前登录的用户及正在运行的程序

```
[root@blog ~]# w
 20:41:44 up 105 days,  6:05,  2 users,  load average: 0.13, 0.10, 0.13
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
root     tty1      318月16 196days  0.02s  0.02s -bash
root     pts/0     20:22    0.00s  0.11s  0.00s w

```

### 查看最近登录用户

```
[root@blog ~]# last
root     pts/0        192.168.1.61     Wed Mar 15 20:22   still logged in
root     pts/1        10.24.12.139     Wed Mar 15 17:00 - 17:42  (00:41)
root     pts/0        10.24.12.139     Wed Mar 15 15:54 - 18:48  (02:54)
root     pts/0        10.24.12.139     Mon Mar 13 10:21 - 16:01  (05:39)
root     pts/0        192.168.1.59     Sun Mar 12 20:16 - 20:29  (00:13)
root     pts/0        10.24.12.62      Fri Mar 10 14:17 - 19:07  (04:49)
```

