---
title: '@解决 Oracle Net Configuration Assistant Failed 问题'
comments: true
categories: guildlines
tags: oracle
abbrlink: 428b54b1
date: 2018-11-13 09:42:32
updated:
keywords:
description:
---

<!--more-->

## 问题描述

现象1：在安装 oracle 12c 时，出现 `Oracle Net Configuration Assistant Failed` 下图
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-181958.png)

现象2：执行 `lsnrctl status`，出现 `TNS-12541`无监听，如：
```shell
[oracle@oem ~]$ lsnrctl status listener
LSNRCTL for Linux: Version 11.1.0.6.0 - Production on 22-NOV-2009 13:18:35
Copyright (c) 1991, 2007, Oracle.  All rights reserved.
Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
TNS-12541: TNS:no listener
 TNS-12560: TNS:protocol adapter error
  TNS-00511: No listener
   Linux Error: 111: Connection refused
```
或者出现`The listener supports no services`，如：
```shell
[oracle@oem ~]$ lsnrctl status

LSNRCTL for Linux: Version 12.1.0.2.0 - Production on 13-NOV-2018 04:10:23

Copyright (c) 1991, 2014, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=oem.com)(PORT=1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
Start Date                13-NOV-2018 04:06:01
Uptime                    0 days 0 hr. 4 min. 22 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/12.1.0/dbhome_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/oem/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=oem.com)(PORT=1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
The listener supports no services
The command completed successfully
```
现象3：`/u01/app/oracle/product/12.1.0/dbhome_1/network/admin/` 目录下没有 `listener.ora` 文件

## 解决方案

在 macos 上打开一个新的终端，运行 `net configuration assistant` 向导程序。执行以下内容：
```shell
ssh -Y oracle@192.168.12.174
oracle@192.168.12.174's password:
Last login: Tue Nov 13 04:04:24 2018
[oracle@oem ~]$ /u01/app/oracle/product/12.1.0/dbhome_1/bin/netca
```
> 注：mac 上需要安装 `xquartz`，否则无法运行 netca GUI 向导界面

然后，按照[此文](http://blog.51cto.com/werewolftj/1591893)一步一步操作即可。
