---
title: SeLinux
date: 2017-01-23 09:56:56
categories: Linux
tags: [Linux,SeLinux]
---
<Excert in index | 首页摘要>
要配置SAMBA服务，结果被SeLinux这玩意给坑了好大一把，这里记录一下SeLinux。其中大部分内容取自《鸟哥的Linux私房菜：基础学习篇》
<!-- more -->
<the rest of contents | 余下全文>

　　SELinux(Security-Enhanced Linux) 是在内核中实践的强制访问控制（MAC）的安全性机制。

　　是 Linux历史上最杰出的新安全子系统。NSA是在Linux社区的帮助下开发了一种访问控制体系，在这种访问控制体系的限制下，进程只能访问那些在他的任务中所需要的文件。

　　从进入了 CentOS 5.x 之后的 CentOS 版本中 （当然包括 CentOS 7），SELinux 已经是个非常完备的核心模块了！尤其 CentOS 提供了很多管理 SELinux 的指令与机制， 因此在整体架构上面是单纯且容易操作管理的！所以，在没有自行开发网络服务软件以及使用其他第三方协力软件的情况下， 也就是全部使用 CentOS 官方提供的软件来使用我们服务器的情况下，建议大家不要关闭 SELinux 了喔！ 让我们来仔细的玩玩这家伙吧！

## 什么是SeLinux
SELinux(Security-Enhanced Linux)，字面撒花姑娘的意义就是安全强化的Linux之意。
### 当初设计的目标是：避免资源的误用
　　当初开发这玩意儿的目的是因为很多企业界发现， 通常系统出现问题的原因大部分都在于“内部员工的资源误用”所导致的，实际由外部发动的攻击反而没有这么严重。 那么什么是“员工资源误用”呢？举例来说，如果有个不是很懂系统的系统管理员为了自己设置的方便，将网页所在目录 /var/www/html/ 的权限设置为drwxrwxrwx 时，你觉得会有什么事情发生？

　　现在我们知道所有的系统资源都是通过程序来进行存取的，那么 /var/www/html/ 如果设置为777 ，代表所有程序均可对该目录存取，万一你真的有启动 WWW 服务器软件，那么该软件所触发的程序将可以写入该目录， 而该程序却是对整个 Internet 提供服务的！只要有心人接触到这支程序，而且该程序刚好又有提供使用者进行写入的功能， 那么外部的人很可能就会对你的系统写入些莫名其妙的东西！那可真是不得了！一个小小的 777 问题可是大大的！
为了控管这方面的权限与程序的问题