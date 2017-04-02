---
title: Linux系统启动方式System-V
date: 2017-01-20 10:02:37
categories: Linux
tags: [Linux,启动模式,System_V]
---

<Excert in index | 首页摘要>
Linux启动模式System V介绍
<!-- more -->
<the rest of contents>

System V启动方式，是Linux采用的启动方式，System V能够为不同的运行级别定义启动那些服务.
进入Linux系统后看到的黑乎乎的界面（开发模式），因为系统如果启动选择开发模式，** 能减少启动时间，优化内存等 **。但是通常刚安装完的Linux系统，看到的是图形化界面。
> CentOS下切换到开发模式，使用快捷键 Ctrl+F2,从开发模式返回图形界面使用Ctrl+F1
> 命令行里输入命令startx命令切换到图形界面

### 系统启动模式配置文件 
#### /etc/inittab
** 设置系统启动模式，必须使用root身份登录才能修改 **
```
[root@admin ~]:# vi /etc/inittab
```
输入后打开inittab文件，看到配置信息如下：
```
\# inittab is only used by upstart for the default runlevel.
\#
\# ADDING OTHER CONFIGURATION HERE WILL HAVE NO EFFECT ON YOUR SYSTEM.
\#
\# System initialization is started by /etc/init/rcS.conf
\#
\# Individual runlevels are started by /etc/init/rc.conf
\#
\# Ctrl-Alt-Delete is handled by /etc/init/control-alt-delete.conf
\#
\# Terminal gettys are handled by /etc/init/tty.conf and /etc/init/serial.conf,
\# with configuration in /etc/sysconfig/init.
\#
\# For information on how to write upstart event handlers, or how
\# upstart works, see init(5), init(8), and initctl(8).
\#
\# Default runlevel. The runlevels used are:
\#   0 - halt (Do NOT set initdefault to this)
\#   1 - Single user mode
\#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
\#   3 - Full multiuser mode
\#   4 - unused
\#   5 - X11
\#   6 - reboot (Do NOT set initdefault to this)
\#
id:5:initdefault:
~   
```

可以看到Linux系统中默认的系统启动基本一共有7种，分别是：

### 0：关机(不要设置这个！)
### 1：单用户(类似于windows操作系统的安全模式)
### 2：多用户状态没有网络服务     
### 3：多用户状态由网络服务(在做开发时，通常设置成这个启动级别，直接进入到命令行的界面)
### 4：系统未使用保留给用户(不要设置这个！)       
### 5：图形界面(这是linux默认的启动级别，直接进入图形界面)
### 6：系统重启(不要设置这个！)
	
最后的一行设置系统默认的启动级别,5，也就是图形界面启动
```
id:5:initdefault:
```
如果需要修改默认级别为开发模式，只需要将5改成3，然后重新启动系统即可
```
将 id:5:initdefault:  改成   id:3:initdefault:
```
** 警告：千万不要将系统基本设置为0,4,6！！！！！！ **

如果只是需要在Linux上做开发、部署项目的话，建议一般讲系统启动模式设置为开发模式

System V启动方式，启动服务的脚本放在/etc/rc.d/init.d下，能够在/etc/rc.d目录下看到很多类似 rc0.d或rc2.d这样的目录，这就是为每个不同的运行级别定义启动哪些服务的目录，数字0 2就代表运行级别，进入这些目录，能看到很多链接文件，以S或K开头的这些文件分别表示在当前运行级别下是否开启这个服务，这些文件分别链接到/etc/rc.d/init.d/下面的很多可执行文件。
** 需要注意，在一些System V启动模式的操作系统上（如RedHat9），除了有/etc/rc.d/init.d/这个目录，还有/etc/init.d/这个目录，其实 ls -l一下就可以看到，/etc/init.d/这个目录本来就是链接到/etc/rc.d/init.d/的一个链接目录 **

```
** 如果有恶意用户将系统启动级别设置成0、4、6，我们该怎么解决这个问题？ **

在linux系统启动界面，我们快速按键盘上的 【e】 按钮，然后进入到了grub引导界面(这个根据Linux的版本可能有不同，我的CentOS6.4是需要在启动时按F2进入引导界面，
这个可以根据自己安装的Linux系统在开机时的提示进入引导界面)，
在这个界面中选择第二个选项，然后再按下键盘上的 【e】按钮，在进入修改界面后，在最后输入【 1】(1前面有空格)
这样，linux系统在启动时就会以 单用户级别 启动起来(为什么这里不将其设置成3或者5，是因为linux系统
在启动时首先会去检查 /etc/inittab 文件的设定启动级别，如果在这时设置成5或者3，系统还是进不去，只能设置成1)
在设置好以后，按下键盘的【b】按钮，系统就能重新启动，并进入 单用户级别，这样我们就可以按照之前的方法修改
linux系统的启动级别。
```