---
title: '@Vmware Funsion8.5中安装 CentOS 7(macOS Sierra)'
comments: true
categories: tutorial
abbrlink: ae6c2890
date: 2017-07-21 22:00:03
updated: 2018-01-18 03:04:56
tags: linux
keywords:
description:
---


## step1: 安装虚拟机 VMware Fusion8

> __VMware 的安装破解非常简单，可以直接下载最新的官方包，在网上基本都能搜到可用的序列号去激活。__

__方式一：__

- 官网下载 [VMware Fusion8 pro](https://my.vmware.com/en/web/vmware/info/slug/desktop_end_user_computing/vmware_fusion/8_0)
- 使用下方的序列号完成激活，若序列号不可用，再使用下方的注册机完成激活。

```

序列号：
FY75A-06W1M-H85PZ-0XP7T-MZ8E8
ZY7TK-A3D4N-08EUZ-TQN5E-XG2TF
FG1MA-25Y1J-H857P-6MZZE-YZAZ6

注册机：
链接: http://pan.baidu.com/s/1t1aMy 密码: 3vkq

```

__方式二：__  
参考：[VMware Fusion 8.5.3 Mac 破解版--史蒂芬周](http://www.sdifen.com/vmwarefusion853.html)

---

## step2: 在虚拟机中安装 Linux 系统(CentOS7)

- 官网下载 [centos-DVD 版](https://www.centos.org/download/)
- 参考[此文](http://www.echojb.com/network/2017/01/22/308045.html)安装
- 建议虚拟机给 2G 内存，60G 以上磁盘空间（由于个人可能会在 linux 中安装 oracle 等大型软件）

另外，对于刚接触 Linux 系统的用户，推荐[鸟哥的 Linux 私房菜](http://linux.vbird.org/linux_basic/) ；也可参考 _鸟哥_ 的[安裝 CentOS7.x](http://linux.vbird.org/linux_basic/0157installcentos7.php)，里面涉及了 Linux 中如何分区的问题，以及 GPT 模式安装。

