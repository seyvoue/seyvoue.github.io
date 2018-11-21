---
title: '@linux 下 cx_Oracle 模块的安装'
comments: true
categories: guildlines
tags: python
abbrlink: 6b43c0d2
date: 2018-09-04 16:03:41
updated:
keywords:
description:
---

python 想远程访问 Oracle 数据库，需要 cx_Oralce 模块，安装该模块前需要预安装：
- oracle instantclient
- python-devel

<!--more-->

## install oracle instantclient

安装 Oracle instantclient，需要下载以下文件：
- instantclient-basic-linux.x64-11.2.0.4.0.zip
- instantclient-sdk-linux.x64-11.2.0.4.0.zip
- instantclient-sqlplus-linux.x64-11.2.0.4.0.zip（可选，该包为命令行工具，类似于 mysql 的交互式命令行界面）

**step1: 下载安装包**
下载链接：`http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html`
根据 linux 系统版本，以及 Oracle 的版本，选择对应版本的安装包，可以使用如下命令，查看系统信息以及版本信息，若服务器上使用的 Oracle 版本较老，建议安装低版本的 instantclient，否则建议使用最新版本。
```shell
[zodas@localhost ~]$ uname -a
[zodas@localhost ~]$ cat /etc/redhat-release
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-060221.png)

**step2: 解压并配置环境变量**

首先，本文使用 zip 包方式安装 instantclient，将所有zip 包都解压到 `/opt/`目录下
```shell
[root@localhost ~]# cd /opt/
[root@localhost opt]# unzip instantclient-basic-linux.x64-18.3.0.0.0dbru.zip
[root@localhost opt]# unzip instantclient-sdk-linux.x64-18.3.0.0.0dbru.zip
[root@localhost opt]# unzip instantclient-sqlplus-linux.x64-18.3.0.0.0dbru.zip
```

其次，增加环境变量，在`~/.bashrc`文件中新增：
```
export ORACLE_HOME=/opt/instantclient_18_3
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME
```

然后，增加软链接
```shell
[root@localhost opt]# cd instantclient_18_3
[root@localhost instantclient_18_3]# ln -s libocci.so.18.1 libocci.so
[root@localhost instantclient_18_3]# ln -s libclntsh.so.18.1 libclntsh.so
```

## install python-devel

下载链接：`https://rpmfind.net/linux/rpm2html/search.php?query=python-devel`

```shell
[root@localhost ~]# yum install python-devel
# 或者使用 rpm 方式
[root@localhost ~]# rpm -ivh python-devel-2.7.5-68.el7.x86_64.rpm
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-074654.png)

## install cx_Oracle

下载链接：`https://pypi.org/project/cx_Oracle/#files`

首先，下载安装
```shell
[root@localhost opt]# tar -xvf cx_Oracle-6.4.1.tar.gz
[root@localhost opt]# cd cx_Oracle-6.4.1
[root@localhost opt]# python setup.py install
```

## 参考链接

- [Python使用cx_Oracle的几个小坑,by bluexiii](https://www.jianshu.com/p/afe71f6e115f)