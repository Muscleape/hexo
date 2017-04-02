---
title: CentOS6.5安装MySQL5.6
date: 2017-03-13 10:17:45
categories: Linux
tags: [Linux,CentOS6.5,MySQL5.6]
---
<Excert in index | 首页摘要>
CentOS6.5安装MySQL5.6，开放防火墙3306端口，允许其他主机使用root账户密码访问MySQL数据库
<!-- more -->
<the rest of contents | 余下全文>
## 查看操作系统相关信息

** 该查看方法只适用于CentOS6.5 （lsb_release -a） **

```
[root@localhost ~]# lsb_release -a
LSB Version:	:base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID:	CentOS
Description:	CentOS release 6.5 (Final)
Release:	6.5
Codename:	Final

```

## 查看当前系统中是否已经安装MySQL

```
[root@localhost ~]# rpm -qa | grep mysql
```

** 执行以上命令后没有任何输出，说明没有安装过MySQL **

## 下载MySQL安装文件

> MySQL5.6下载地址：https://dev.mysql.com/downloads/mysql/5.6.html#downloads

需要下载的三个安装包：
- MySQL-client-5.6.35-1.el6.x86_64.rpm
- MySQL-devel-5.6.35-1.el6.x86_64.rpm
- MySQL-server-5.6.35-1.el6.x86_64.rpm

创建一个目录，存放下载的安装文件：

```
[root@localhost mysql]# ll
总用量 78016
-rw-r--r--. 1 root root 19012192 3月  13 18:07 MySQL-client-5.6.35-1.el6.x86_64.rpm
-rw-r--r--. 1 root root  3423496 3月  13 18:06 MySQL-devel-5.6.35-1.el6.x86_64.rpm
-rw-r--r--. 1 root root 57446932 3月  13 18:08 MySQL-server-5.6.35-1.el6.x86_64.rpm

```

## 安装下载的rpm软件包

### 安装server.rpm
```
[root@localhost mysql]# rpm -ivh MySQL-server-5.6.35-1.el6.x86_64.rpm 
warning: MySQL-server-5.6.35-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                ########################################### [100%]
   1:MySQL-server           ########################################### [100%]
***************************************
******* 中间部分还有一些输出信息，忽略显示 *******
***************************************
A RANDOM PASSWORD HAS BEEN SET FOR THE MySQL root USER !
You will find that password in '/root/.mysql_secret'.

You must change that password on your first connect,
no other statement but 'SET PASSWORD' will be accepted.
See the manual for the semantics of the 'password expired' flag.

Also, the account for the anonymous user has been removed.

In addition, you can run:

  /usr/bin/mysql_secure_installation

which will also give you the option of removing the test database.
This is strongly recommended for production servers.

See the manual for more instructions.

Please report any problems at http://bugs.mysql.com/

The latest information about MySQL is available on the web at

  http://www.mysql.com

Support MySQL by buying support/licenses at http://shop.mysql.com

New default config file was created as /usr/my.cnf and
will be used by default by the server when you start it.
You may edit this file to change server settings
```

### 安装client.rpm

```
[root@localhost mysql]# rpm -ivh MySQL-client-5.6.35-1.el6.x86_64.rpm 
warning: MySQL-client-5.6.35-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                ########################################### [100%]
   1:MySQL-client           ########################################### [100%]
```

### 安装devel.rpm

```
[root@localhost mysql]# rpm -ivh MySQL-devel-5.6.35-1.el6.x86_64.rpm 
warning: MySQL-devel-5.6.35-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Preparing...                ########################################### [100%]
   1:MySQL-devel            ########################################### [100%]
```

## 修改配置文件位置

```
[root@localhost mysql]# cp my-default.cnf /etc/my.cnf
```

## 启动MySQL服务

```
[root@localhost etc]# service mysql start
Starting MySQL                                             [确定]
```

## 初始化MySQL及修改MySQL默认的root密码

### 查看MySQL进程

```
[root@localhost etc]# ps -ef | grep mysql
root      6692     1  0 18:46 pts/1    00:00:00 /bin/sh /usr/bin/mysqld_safe --datadir=/var/lib/mysql --pid-file=/var/lib/mysql/localhost.localdomain.pid
mysql     6800  6692  0 18:46 pts/1    00:00:00 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --user=mysql --log-error=/var/lib/mysql/localhost.localdomain.err --pid-file=/var/lib/mysql/localhost.localdomain.pid
root      7571  6285  0 19:11 pts/1    00:00:00 grep mysql

```

### 查看3306端口的占用

```
[root@localhost etc]# netstat -anpt | grep 3306
tcp        0      0 :::3306                     :::*                        LISTEN      6800/mysqld 
```
### 查看root用户的默认密码

```
[root@localhost mysql]# more /root/.mysql_secret
# The random password set for the root user at Mon Mar 13 18:09:24 2017 (local time): 5dfEph_ZVzlqnEIC

```

### 使用默认密码登录MySQL

** 密码使用7.3中看到的默认密码 **

```
[root@localhost mysql]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.6.35

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

```
在MySQL中重置root用户密码

```
mysql> SET PASSWORD = PASSWORD('aaaaaa');
Query OK, 0 rows affected (0.00 sec)

```

退出MySQL

```
mysql> exit
Bye

```

## 设置MySQL服务开机自启动

```
[root@localhost mysql]# chkconfig mysql on
[root@localhost mysql]# chkconfig mysql --list
mysql          	0:关闭	1:关闭	2:启用	3:启用	4:启用	5:启用	6:关闭

```

## 授权用户从其他主机连接MySQL服务

### 允许root使用密码从任何主机连接到MySQL

> GRANT ALL PRIVILEGES ON *.* TO 'root'@'允许访问的主机的IP' IDENTIFIED BY 'root用户密码' WITH GRANT  OPTION;

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'aaaaaa' WITH GRANT  OPTION;
Query OK, 0 rows affected (0.00 sec)
```

### 允许root使用密码从固定某一台主机（192.168.1.59）连接到MySQL

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.1.59' IDENTIFIED BY 'aaaaaa' WITH GRANT  OPTION;
Query OK, 0 rows affected (0.00 sec)
```

### 修改生效

```
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```

## 开放3306端口

### 查看iptable防火墙的现有配置

没有开放3306端口

```
[root@localhost mysql]# service iptables status
表格：filter
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED 
2    ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           
3    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:22 
5    REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited 

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination         
1    REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited 

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination 
```

### 修改防火墙配置，添加3306端口

```
[root@localhost mysql]# vi /etc/sysconfig/iptables
```
** 加入以下防火墙配置节，需要添加到默认的22号端口这条规则的下面 **

> -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

查看修改后的防火墙配置信息

```
[root@localhost mysql]# cat /etc/sysconfig/iptables
# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT

```

### 重启防火墙使配置生效

```
[root@localhost mysql]# service iptables restart
iptables：将链设置为政策 ACCEPT：filter                    [确定]
iptables：清除防火墙规则：                                 [确定]
iptables：正在卸载模块：                                   [确定]
iptables：应用防火墙规则：                                 [确定]

```

### 查看是否已开放3306端口

```
[root@localhost mysql]# service iptables status
表格：filter
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED 
2    ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           
3    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:22 
5    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:3306 
6    REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited 

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination         
1    REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited 

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination   
```