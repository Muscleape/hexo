---
title: 虚拟机体积压缩
date: 2017-01-22 09:39:11
categories: VirtualMachine
tags: [vmdk,VirtualBox,VMWare,虚拟机]
---
<Excert in index | 首页摘要>
压缩vmdk格式虚拟机文件体积
<!-- more -->
<the rest of contents | 余下全文>

### 简述
　　虚拟机在使用一段时间之后，体积会越来越大，导出或共享虚拟机十分麻烦。启动虚拟机，删除没啥用的文件后，从虚拟机里看磁盘占用小了很多，但是物理机磁盘上存放的虚拟机的体积并没有减小，在虚拟机里面删除文件后，虚拟机软件不会对虚拟磁盘进行“压缩”，使用该博客提到的方法能够有效压缩vmdk格式虚拟机体积。

### 需要使用的工具： DiskGenius

### 操作过程
1、在DiskGenius中打开需要压缩的虚拟磁盘（菜单：【硬盘】-【打开虚拟硬盘文件】）。然后就可以在窗口中看到加载上的虚拟磁盘了。
2、新建一个容量不小于1中打开的文件的虚拟磁盘（菜单：【硬盘】-【新建虚拟硬盘文件】-【新建VMWare虚拟硬盘文件】）。
3、开始进行压缩。选择（菜单：【工具】-【克隆硬盘】），在弹出的对话框中，【选择源硬盘】：选择1中打开的需要压缩的vmdk虚拟磁盘，【选择目标硬盘】：选择2中新建的vmdk虚拟磁盘，勾选【按文件复制（可消除碎片）】。
4、将源硬盘文件改名，然后将新硬盘文件的名称改为与源硬盘文件原来的名称一致（** 注意：需要完全一致 **）。打开虚拟机，启动后，新虚拟机能正常使用，则可以删除源虚拟硬盘文件了。