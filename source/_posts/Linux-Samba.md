---
title: Linux与Windows资源共享服务Samba
date: 2017-01-22 11:02:14
categories: Linux
tags: [Linux,Samba,共享]
---
<Excert in the index | 首页摘要>
Linux系统需要与Windows系统之前共享文件，实现Windows能访问Linux的目录，存放项目文件，自动部署项目到Linux系统。Windows对Linux目录中的文件只有读的权限。
<!-- more -->
<the rest of contents | 余下全文>

## SAMBA简介
　　Samba，是种用来让UNIX系列的操作系统与微软Windows操作系统的SMB/CIFS（Server Message Block/Common Internet File System）网络协议做链接的自由软件。第三版不仅可访问及分享SMB的文件夹及打印机，本身还可以集成入Windows Server的网域，扮演为网域控制站（Domain Controller）以及加入Active Directory成员。简而言之，此软件在Windows与UNIX系列OS之间搭起一座桥梁，让两者的资源可互通有无。

## 环境配置

　　由于防火墙（CnetOS7默认使用firewalld）及 SELinux 保护，在 CentOS 上设置 Samba 是比较难。这其实是一件好事，因为安全性是非常重要，但要令 Samba 与服务器的外界沟通，我们需要花点功夫及对它有所认识。

　　SAMBA 采用端口 137 — 139 及 445。为什么会是这些端口？让我们看得再详细一点。微软的旧款主从通讯协议是 netbios。它的端口是 137、138 及 139。这款网络设置依赖一台 netbios 服务器（WINS即Windows Internet Naming Service）来提供传送给客户端的名称。换句话说，WINS 就是昔日的 DNS。它可以是一项非常不安全的服务，但设置却很容易。如果你曾经采用过 net view 或 net use 这些指令，你便是使用 WINS 服务。

　　时移世易，WINS 被新进的 Active Directory（AD）所淘汰。这是微软对 Novell 的 Networking 服务（NDS）的反击，而它又是 Novell 对 UNIX 的 NFS 服务器的反应。最大的分别就是 Active Directory 依赖一台 DNS 服务器而不是 netbios。这意味着端口上的改动。微软的 AD 服务改用 445 号端口（UDP 及 TCP）。现时，除非你就这个服务拥有一台 Windows 服务器，否则 Windows 缺省会采用 netbios，而你必须通过控制台内的「系统」图示进行特别设置才能更改它。如果你不会采用 Netbios，你亦可以在 Windows 控制台的「网络连接」图示下的 tcp/ip 服务来停用它。再一次，这些设置都超越了这份文档的范畴。

　　现在让我们再次返回那些端口及 Samba。以下的清单详细列出 Samba 在你的系统上运行时所需的端口。请注意一个 TCP 端口只是个服务端口。80 号端口是供网页使用，而 22 号端口是供安全远程连接（ssh）使用。UDP（用户定义端口）是变种的 TCP 端口。这篇文章不会以解释当中的分别作为焦点，但你可以轻易地在网上寻找。现时你只需知道它们是有分别的。

** （旧的系统） **

- 137 号端口 —— UDP NetBIOS 命名服务（WINS）
- 138 号端口 —— UDP NetBIOS 数据包
- 139 号端口 —— TCP NetBIOS 工作阶段（TCP）、Windows 文件及打印机共享（这是最不安全的端口）

** （Active Directory）  **

- 445 号端口 —— Microsoft-DS Active Directory、Windows 共享资源（TCP）
- 445 号端口 —— Microsoft-DS SMB 文件共享（UDP）

为何要知道这一切呢？因为你须要知道为 SAMBA 打开及不要打开哪些端口，否则你便不能令它在 CentOS 上运作。

### 防火墙设置

### SeLinux配置
生产环境建议启用