---
title: '@在 centos7 下安装 oracle 12c'
comments: true
categories: guildlines
tags: oracle
abbrlink: e2a845a8
date: 2018-11-07 12:43:19
updated:
keywords:
description:
---

> 图形界面方式在 centos 7 下安装 oracle 12c R1

## 环境

- VM: VMware Fusion 8.5
- hostname: localhost.localdomain
- ip: 192.168.12.xxx
- OS: CentOS Linux 7 (Core)
    - Memory: 4G (不小于4G)
    - HDD: 100G
    - /swap: 4G(不小于4G)
    - /: 50G（最好大于40G）
- DB: Oracle Database 12c Release 1(12.1.0.2.0) - Enterprise Edition for Linux x86-64 （server class）
- EM: Oracle Enterprise Manager Cloud Control 12c Release 4 (12.1.0.4) for Linux x86-64

<!--more-->

## 安装必须的软件包

需要连接外网，从Oracle Public Yum仓库来安装`oracle-rdbms-server-12cR1-preinstall`

先下载 repo 文件

```shell
[root@localhost zodas]# cd /etc/yum.repos.d/
[root@localhost yum.repos.d]# wget http://public-yum.oracle.com/public-yum-ol7.repo
--2018-11-12 20:20:20--  http://public-yum.oracle.com/public-yum-ol7.repo
Resolving public-yum.oracle.com (public-yum.oracle.com)... 23.35.178.109
Connecting to public-yum.oracle.com (public-yum.oracle.com)|23.35.178.109|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14602 (14K) [text/plain]
Saving to: ‘public-yum-ol7.repo’

100%[========================================================================================================================================>] 14,602      --.-K/s   in 0s

2018-11-12 20:20:21 (197 MB/s) - ‘public-yum-ol7.repo’ saved [14602/14602]
```

测试 yum 是否正常工作
```shell
[root@localhost yum.repos.d]# yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.njupt.edu.cn
 * extras: mirrors.aliyun.com
 * updates: mirrors.njupt.edu.cn
ol7_UEKR4                                                                                                                                                  | 1.2 kB  00:00:00
ol7_latest                                                                                                                                                 | 1.4 kB  00:00:00
(1/5): ol7_UEKR4/x86_64/updateinfo                                                                                                                         |  82 kB  00:00:00
(2/5): ol7_latest/x86_64/group                                                                                                                             | 659 kB  00:00:00
(3/5): ol7_UEKR4/x86_64/primary                                                                                                                            | 3.5 MB  00:00:01
(4/5): ol7_latest/x86_64/primary                                                                                                                           | 9.5 MB  00:00:04
(5/5): ol7_latest/x86_64/updateinfo                                                                                                                        | 740 kB  00:00:19
ol7_UEKR4                                                                                                                                                                 124/124
ol7_latest                                                                                                                                                            11482/11482
repo id                                                  repo name                                                                                                          status
base/7/x86_64                                            CentOS-7 - Base                                                                                                     9,911
extras/7/x86_64                                          CentOS-7 - Extras                                                                                                     434
ol7_UEKR4/x86_64                                         Latest Unbreakable Enterprise Kernel Release 4 for Oracle Linux 7 (x86_64)                                            124
ol7_latest/x86_64                                        Oracle Linux 7 Latest (x86_64)                                                                                     11,482
updates/7/x86_64                                         CentOS-7 - Updates                                                                                                  1,614
repolist: 23,565
```

下载 `RPM-GPG-KEY-oracle` 
```shell
[root@localhost yum.repos.d]# wget https://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol7 -O /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
--2018-11-12 20:22:51--  https://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol7
Resolving public-yum.oracle.com (public-yum.oracle.com)... 23.51.208.99
Connecting to public-yum.oracle.com (public-yum.oracle.com)|23.51.208.99|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1011 [text/plain]
Saving to: ‘/etc/pki/rpm-gpg/RPM-GPG-KEY-oracle’

100%[========================================================================================================================================>] 1,011       --.-K/s   in 0s

2018-11-12 20:22:53 (127 MB/s) - ‘/etc/pki/rpm-gpg/RPM-GPG-KEY-oracle’ saved [1011/1011]
```

安装 `oracle-rdbms-server-12cR1-preinstall`

<details><summary>点击显/隐内容</summary>
<p>
```shell
[root@localhost yum.repos.d]# yum install oracle-rdbms-server-12cR1-preinstall
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.njupt.edu.cn
 * extras: mirrors.aliyun.com
 * updates: mirrors.njupt.edu.cn
Resolving Dependencies
--> Running transaction check
---> Package oracle-rdbms-server-12cR1-preinstall.x86_64 0:1.0-7.el7 will be installed
--> Processing Dependency: bc for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: gcc for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: sysstat for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: xorg-x11-utils for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: bind-utils for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: gcc-c++ for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: kernel-uek for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: compat-libcap1 for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: ksh for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: libaio-devel for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: xorg-x11-xauth for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: psmisc for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: unzip for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: glibc-devel for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: compat-libstdc++-33 for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: libstdc++-devel for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: nfs-utils for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Processing Dependency: smartmontools for package: oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64
--> Running transaction check
---> Package bc.x86_64 0:1.06.95-13.el7 will be installed
---> Package bind-utils.x86_64 32:9.9.4-72.el7 will be installed
--> Processing Dependency: bind-libs = 32:9.9.4-72.el7 for package: 32:bind-utils-9.9.4-72.el7.x86_64
--> Processing Dependency: libisccc.so.90()(64bit) for package: 32:bind-utils-9.9.4-72.el7.x86_64
--> Processing Dependency: liblwres.so.90()(64bit) for package: 32:bind-utils-9.9.4-72.el7.x86_64
--> Processing Dependency: libbind9.so.90()(64bit) for package: 32:bind-utils-9.9.4-72.el7.x86_64
--> Processing Dependency: libisc.so.95()(64bit) for package: 32:bind-utils-9.9.4-72.el7.x86_64
--> Processing Dependency: libdns.so.100()(64bit) for package: 32:bind-utils-9.9.4-72.el7.x86_64
--> Processing Dependency: libisccfg.so.90()(64bit) for package: 32:bind-utils-9.9.4-72.el7.x86_64
---> Package compat-libcap1.x86_64 0:1.10-7.el7 will be installed
---> Package compat-libstdc++-33.x86_64 0:3.2.3-72.el7 will be installed
---> Package gcc.x86_64 0:4.8.5-36.0.1.el7 will be installed
--> Processing Dependency: libgomp = 4.8.5-36.0.1.el7 for package: gcc-4.8.5-36.0.1.el7.x86_64
--> Processing Dependency: cpp = 4.8.5-36.0.1.el7 for package: gcc-4.8.5-36.0.1.el7.x86_64
--> Processing Dependency: libgcc >= 4.8.5-36.0.1.el7 for package: gcc-4.8.5-36.0.1.el7.x86_64
--> Processing Dependency: libmpfr.so.4()(64bit) for package: gcc-4.8.5-36.0.1.el7.x86_64
--> Processing Dependency: libmpc.so.3()(64bit) for package: gcc-4.8.5-36.0.1.el7.x86_64
---> Package gcc-c++.x86_64 0:4.8.5-36.0.1.el7 will be installed
--> Processing Dependency: libstdc++ = 4.8.5-36.0.1.el7 for package: gcc-c++-4.8.5-36.0.1.el7.x86_64
---> Package glibc-devel.x86_64 0:2.17-260.0.9.el7 will be installed
--> Processing Dependency: glibc-headers = 2.17-260.0.9.el7 for package: glibc-devel-2.17-260.0.9.el7.x86_64
--> Processing Dependency: glibc = 2.17-260.0.9.el7 for package: glibc-devel-2.17-260.0.9.el7.x86_64
--> Processing Dependency: glibc-headers for package: glibc-devel-2.17-260.0.9.el7.x86_64
---> Package kernel-container.x86_64 0:3.10.0-0.0.0.2.el7 will be installed
---> Package ksh.x86_64 0:20120801-139.0.1.el7 will be installed
---> Package libaio-devel.x86_64 0:0.3.109-13.el7 will be installed
---> Package libstdc++-devel.x86_64 0:4.8.5-36.0.1.el7 will be installed
---> Package nfs-utils.x86_64 1:1.3.0-0.61.0.1.el7 will be installed
--> Processing Dependency: gssproxy >= 0.7.0-3 for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: libtirpc >= 0.2.4-0.7 for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: rpcbind for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: keyutils for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: quota for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: libevent for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: libnfsidmap for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: libevent-2.0.so.5()(64bit) for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: libtirpc.so.1()(64bit) for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
--> Processing Dependency: libnfsidmap.so.0()(64bit) for package: 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64
---> Package psmisc.x86_64 0:22.20-15.el7 will be installed
---> Package smartmontools.x86_64 1:6.5-1.el7 will be installed
--> Processing Dependency: mailx for package: 1:smartmontools-6.5-1.el7.x86_64
---> Package sysstat.x86_64 0:10.1.5-17.el7 will be installed
--> Processing Dependency: libsensors.so.4()(64bit) for package: sysstat-10.1.5-17.el7.x86_64
---> Package unzip.x86_64 0:6.0-19.el7 will be installed
---> Package xorg-x11-utils.x86_64 0:7.5-23.el7 will be installed
--> Processing Dependency: libXext.so.6()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXv.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXtst.so.6()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXrender.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXxf86vm.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXxf86misc.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libX11-xcb.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXrandr.so.2()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libdmx.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXinerama.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libX11.so.6()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXxf86dga.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libxcb.so.1()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libxcb-shape.so.0()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
--> Processing Dependency: libXi.so.6()(64bit) for package: xorg-x11-utils-7.5-23.el7.x86_64
---> Package xorg-x11-xauth.x86_64 1:1.0.9-1.el7 will be installed
--> Processing Dependency: libXmuu.so.1()(64bit) for package: 1:xorg-x11-xauth-1.0.9-1.el7.x86_64
--> Processing Dependency: libXau.so.6()(64bit) for package: 1:xorg-x11-xauth-1.0.9-1.el7.x86_64
--> Running transaction check
---> Package bind-libs.x86_64 32:9.9.4-72.el7 will be installed
--> Processing Dependency: bind-license = 32:9.9.4-72.el7 for package: 32:bind-libs-9.9.4-72.el7.x86_64
---> Package cpp.x86_64 0:4.8.5-36.0.1.el7 will be installed
---> Package glibc.x86_64 0:2.17-222.el7 will be updated
--> Processing Dependency: glibc = 2.17-222.el7 for package: glibc-common-2.17-222.el7.x86_64
---> Package glibc.x86_64 0:2.17-260.0.9.el7 will be an update
---> Package glibc-headers.x86_64 0:2.17-260.0.9.el7 will be installed
--> Processing Dependency: kernel-headers >= 2.2.1 for package: glibc-headers-2.17-260.0.9.el7.x86_64
--> Processing Dependency: kernel-headers for package: glibc-headers-2.17-260.0.9.el7.x86_64
---> Package gssproxy.x86_64 0:0.7.0-21.el7 will be installed
--> Processing Dependency: libini_config >= 1.3.1-31 for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libref_array.so.1(REF_ARRAY_0.1.1)(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libini_config.so.3(INI_CONFIG_1.1.0)(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libini_config.so.3(INI_CONFIG_1.2.0)(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libverto-module-base for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libini_config.so.3()(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libbasicobjects.so.0()(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libref_array.so.1()(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
--> Processing Dependency: libcollection.so.2()(64bit) for package: gssproxy-0.7.0-21.el7.x86_64
---> Package keyutils.x86_64 0:1.5.8-3.el7 will be installed
---> Package libX11.x86_64 0:1.6.5-2.el7 will be installed
--> Processing Dependency: libX11-common >= 1.6.5-2.el7 for package: libX11-1.6.5-2.el7.x86_64
---> Package libXau.x86_64 0:1.0.8-2.1.el7 will be installed
---> Package libXext.x86_64 0:1.3.3-3.el7 will be installed
---> Package libXi.x86_64 0:1.7.9-1.el7 will be installed
---> Package libXinerama.x86_64 0:1.1.3-2.1.el7 will be installed
---> Package libXmu.x86_64 0:1.1.2-2.el7 will be installed
--> Processing Dependency: libXt.so.6()(64bit) for package: libXmu-1.1.2-2.el7.x86_64
---> Package libXrandr.x86_64 0:1.5.1-2.el7 will be installed
---> Package libXrender.x86_64 0:0.9.10-1.el7 will be installed
---> Package libXtst.x86_64 0:1.2.3-1.el7 will be installed
---> Package libXv.x86_64 0:1.0.11-1.el7 will be installed
---> Package libXxf86dga.x86_64 0:1.1.4-2.1.el7 will be installed
---> Package libXxf86misc.x86_64 0:1.0.3-7.1.el7 will be installed
---> Package libXxf86vm.x86_64 0:1.1.4-1.el7 will be installed
---> Package libdmx.x86_64 0:1.1.3-3.el7 will be installed
---> Package libevent.x86_64 0:2.0.21-4.el7 will be installed
---> Package libgcc.x86_64 0:4.8.5-28.el7_5.1 will be updated
---> Package libgcc.x86_64 0:4.8.5-36.0.1.el7 will be an update
---> Package libgomp.x86_64 0:4.8.5-28.el7_5.1 will be updated
---> Package libgomp.x86_64 0:4.8.5-36.0.1.el7 will be an update
---> Package libmpc.x86_64 0:1.0.1-3.el7 will be installed
---> Package libnfsidmap.x86_64 0:0.25-19.el7 will be installed
---> Package libstdc++.x86_64 0:4.8.5-28.el7_5.1 will be updated
---> Package libstdc++.x86_64 0:4.8.5-36.0.1.el7 will be an update
---> Package libtirpc.x86_64 0:0.2.4-0.15.el7 will be installed
---> Package libxcb.x86_64 0:1.13-1.el7 will be installed
---> Package lm_sensors-libs.x86_64 0:3.4.0-6.20160601gitf9185e5.el7 will be installed
---> Package mailx.x86_64 0:12.5-19.el7 will be installed
---> Package mpfr.x86_64 0:3.1.1-4.el7 will be installed
---> Package quota.x86_64 1:4.01-17.el7 will be installed
--> Processing Dependency: quota-nls = 1:4.01-17.el7 for package: 1:quota-4.01-17.el7.x86_64
--> Processing Dependency: tcp_wrappers for package: 1:quota-4.01-17.el7.x86_64
---> Package rpcbind.x86_64 0:0.2.0-47.el7 will be installed
--> Running transaction check
---> Package bind-license.noarch 32:9.9.4-61.el7_5.1 will be updated
--> Processing Dependency: bind-license = 32:9.9.4-61.el7_5.1 for package: 32:bind-libs-lite-9.9.4-61.el7_5.1.x86_64
---> Package bind-license.noarch 32:9.9.4-72.el7 will be an update
---> Package glibc-common.x86_64 0:2.17-222.el7 will be updated
---> Package glibc-common.x86_64 0:2.17-260.0.9.el7 will be an update
---> Package kernel-headers.x86_64 0:3.10.0-957.el7 will be installed
---> Package libX11-common.noarch 0:1.6.5-2.el7 will be installed
---> Package libXt.x86_64 0:1.1.5-3.el7 will be installed
--> Processing Dependency: libSM.so.6()(64bit) for package: libXt-1.1.5-3.el7.x86_64
--> Processing Dependency: libICE.so.6()(64bit) for package: libXt-1.1.5-3.el7.x86_64
---> Package libbasicobjects.x86_64 0:0.1.1-32.el7 will be installed
---> Package libcollection.x86_64 0:0.7.0-32.el7 will be installed
---> Package libini_config.x86_64 0:1.3.1-32.el7 will be installed
--> Processing Dependency: libpath_utils.so.1(PATH_UTILS_0.2.1)(64bit) for package: libini_config-1.3.1-32.el7.x86_64
--> Processing Dependency: libpath_utils.so.1()(64bit) for package: libini_config-1.3.1-32.el7.x86_64
---> Package libref_array.x86_64 0:0.1.5-32.el7 will be installed
---> Package libverto-libevent.x86_64 0:0.2.5-4.el7 will be installed
---> Package quota-nls.noarch 1:4.01-17.el7 will be installed
---> Package tcp_wrappers.x86_64 0:7.6-77.el7 will be installed
--> Running transaction check
---> Package bind-libs-lite.x86_64 32:9.9.4-61.el7_5.1 will be updated
---> Package bind-libs-lite.x86_64 32:9.9.4-72.el7 will be an update
---> Package libICE.x86_64 0:1.0.9-9.el7 will be installed
---> Package libSM.x86_64 0:1.2.2-2.el7 will be installed
---> Package libpath_utils.x86_64 0:0.2.1-32.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==================================================================================================================================================================================
 Package                                                   Arch                        Version                                              Repository                       Size
==================================================================================================================================================================================
Installing:
 oracle-rdbms-server-12cR1-preinstall                      x86_64                      1.0-7.el7                                            ol7_latest                       21 k
Installing for dependencies:
 bc                                                        x86_64                      1.06.95-13.el7                                       base                            115 k
 bind-libs                                                 x86_64                      32:9.9.4-72.el7                                      ol7_latest                      1.0 M
 bind-utils                                                x86_64                      32:9.9.4-72.el7                                      ol7_latest                      205 k
 compat-libcap1                                            x86_64                      1.10-7.el7                                           base                             19 k
 compat-libstdc++-33                                       x86_64                      3.2.3-72.el7                                         base                            191 k
 cpp                                                       x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                      5.9 M
 gcc                                                       x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                       16 M
 gcc-c++                                                   x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                      7.2 M
 glibc-devel                                               x86_64                      2.17-260.0.9.el7                                     ol7_latest                      1.1 M
 glibc-headers                                             x86_64                      2.17-260.0.9.el7                                     ol7_latest                      684 k
 gssproxy                                                  x86_64                      0.7.0-21.el7                                         ol7_latest                      108 k
 kernel-container                                          x86_64                      3.10.0-0.0.0.2.el7                                   ol7_latest                      2.6 k
 kernel-headers                                            x86_64                      3.10.0-957.el7                                       ol7_latest                      8.0 M
 keyutils                                                  x86_64                      1.5.8-3.el7                                          base                             54 k
 ksh                                                       x86_64                      20120801-139.0.1.el7                                 ol7_latest                      883 k
 libICE                                                    x86_64                      1.0.9-9.el7                                          base                             66 k
 libSM                                                     x86_64                      1.2.2-2.el7                                          base                             39 k
 libX11                                                    x86_64                      1.6.5-2.el7                                          ol7_latest                      606 k
 libX11-common                                             noarch                      1.6.5-2.el7                                          ol7_latest                      163 k
 libXau                                                    x86_64                      1.0.8-2.1.el7                                        base                             29 k
 libXext                                                   x86_64                      1.3.3-3.el7                                          base                             39 k
 libXi                                                     x86_64                      1.7.9-1.el7                                          base                             40 k
 libXinerama                                               x86_64                      1.1.3-2.1.el7                                        base                             14 k
 libXmu                                                    x86_64                      1.1.2-2.el7                                          base                             71 k
 libXrandr                                                 x86_64                      1.5.1-2.el7                                          base                             27 k
 libXrender                                                x86_64                      0.9.10-1.el7                                         base                             26 k
 libXt                                                     x86_64                      1.1.5-3.el7                                          base                            173 k
 libXtst                                                   x86_64                      1.2.3-1.el7                                          base                             20 k
 libXv                                                     x86_64                      1.0.11-1.el7                                         base                             18 k
 libXxf86dga                                               x86_64                      1.1.4-2.1.el7                                        base                             19 k
 libXxf86misc                                              x86_64                      1.0.3-7.1.el7                                        base                             19 k
 libXxf86vm                                                x86_64                      1.1.4-1.el7                                          base                             18 k
 libaio-devel                                              x86_64                      0.3.109-13.el7                                       base                             13 k
 libbasicobjects                                           x86_64                      0.1.1-32.el7                                         ol7_latest                       25 k
 libcollection                                             x86_64                      0.7.0-32.el7                                         ol7_latest                       41 k
 libdmx                                                    x86_64                      1.1.3-3.el7                                          base                             16 k
 libevent                                                  x86_64                      2.0.21-4.el7                                         base                            214 k
 libini_config                                             x86_64                      1.3.1-32.el7                                         ol7_latest                       63 k
 libmpc                                                    x86_64                      1.0.1-3.el7                                          base                             51 k
 libnfsidmap                                               x86_64                      0.25-19.el7                                          base                             50 k
 libpath_utils                                             x86_64                      0.2.1-32.el7                                         ol7_latest                       28 k
 libref_array                                              x86_64                      0.1.5-32.el7                                         ol7_latest                       27 k
 libstdc++-devel                                           x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                      1.5 M
 libtirpc                                                  x86_64                      0.2.4-0.15.el7                                       ol7_latest                       88 k
 libverto-libevent                                         x86_64                      0.2.5-4.el7                                          base                            8.9 k
 libxcb                                                    x86_64                      1.13-1.el7                                           ol7_latest                      213 k
 lm_sensors-libs                                           x86_64                      3.4.0-6.20160601gitf9185e5.el7                       ol7_latest                       41 k
 mailx                                                     x86_64                      12.5-19.el7                                          base                            245 k
 mpfr                                                      x86_64                      3.1.1-4.el7                                          base                            203 k
 nfs-utils                                                 x86_64                      1:1.3.0-0.61.0.1.el7                                 ol7_latest                      410 k
 psmisc                                                    x86_64                      22.20-15.el7                                         base                            141 k
 quota                                                     x86_64                      1:4.01-17.el7                                        base                            179 k
 quota-nls                                                 noarch                      1:4.01-17.el7                                        base                             90 k
 rpcbind                                                   x86_64                      0.2.0-47.el7                                         ol7_latest                       59 k
 smartmontools                                             x86_64                      1:6.5-1.el7                                          base                            460 k
 sysstat                                                   x86_64                      10.1.5-17.el7                                        ol7_latest                      314 k
 tcp_wrappers                                              x86_64                      7.6-77.el7                                           base                             78 k
 unzip                                                     x86_64                      6.0-19.el7                                           base                            170 k
 xorg-x11-utils                                            x86_64                      7.5-23.el7                                           ol7_latest                      114 k
 xorg-x11-xauth                                            x86_64                      1:1.0.9-1.el7                                        base                             30 k
Updating for dependencies:
 bind-libs-lite                                            x86_64                      32:9.9.4-72.el7                                      ol7_latest                      740 k
 bind-license                                              noarch                      32:9.9.4-72.el7                                      ol7_latest                       86 k
 glibc                                                     x86_64                      2.17-260.0.9.el7                                     ol7_latest                      3.6 M
 glibc-common                                              x86_64                      2.17-260.0.9.el7                                     ol7_latest                       11 M
 libgcc                                                    x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                      101 k
 libgomp                                                   x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                      157 k
 libstdc++                                                 x86_64                      4.8.5-36.0.1.el7                                     ol7_latest                      304 k

Transaction Summary
==================================================================================================================================================================================
Install  1 Package  (+60 Dependent packages)
Upgrade             (  7 Dependent packages)

Total download size: 64 M
Is this ok [y/d/N]: y
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/68): bc-1.06.95-13.el7.x86_64.rpm                                                                                                                       | 115 kB  00:00:00
warning: /var/cache/yum/x86_64/7/ol7_latest/packages/bind-libs-lite-9.9.4-72.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID ec551f03: NOKEY57 kB/s | 502 kB  00:02:22 ETA
Public key for bind-libs-lite-9.9.4-72.el7.x86_64.rpm is not installed
(2/68): bind-libs-lite-9.9.4-72.el7.x86_64.rpm                                                                                                             | 740 kB  00:00:01
(3/68): bind-libs-9.9.4-72.el7.x86_64.rpm                                                                                                                  | 1.0 MB  00:00:01
(4/68): compat-libcap1-1.10-7.el7.x86_64.rpm                                                                                                               |  19 kB  00:00:00
(5/68): bind-utils-9.9.4-72.el7.x86_64.rpm                                                                                                                 | 205 kB  00:00:00
(6/68): bind-license-9.9.4-72.el7.noarch.rpm                                                                                                               |  86 kB  00:00:00
(7/68): compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm                                                                                                        | 191 kB  00:00:00
(8/68): cpp-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                                    | 5.9 MB  00:00:01
(9/68): gcc-c++-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                                | 7.2 MB  00:00:02
(10/68): glibc-2.17-260.0.9.el7.x86_64.rpm                                                                                                                 | 3.6 MB  00:00:01
(11/68): glibc-common-2.17-260.0.9.el7.x86_64.rpm                                                                                                          |  11 MB  00:00:02
(12/68): glibc-devel-2.17-260.0.9.el7.x86_64.rpm                                                                                                           | 1.1 MB  00:00:00
(13/68): glibc-headers-2.17-260.0.9.el7.x86_64.rpm                                                                                                         | 684 kB  00:00:00
(14/68): gssproxy-0.7.0-21.el7.x86_64.rpm                                                                                                                  | 108 kB  00:00:00
(15/68): kernel-container-3.10.0-0.0.0.2.el7.x86_64.rpm                                                                                                    | 2.6 kB  00:00:00
(16/68): keyutils-1.5.8-3.el7.x86_64.rpm                                                                                                                   |  54 kB  00:00:00
(17/68): kernel-headers-3.10.0-957.el7.x86_64.rpm                                                                                                          | 8.0 MB  00:00:01
(18/68): libICE-1.0.9-9.el7.x86_64.rpm                                                                                                                     |  66 kB  00:00:00
(19/68): libSM-1.2.2-2.el7.x86_64.rpm                                                                                                                      |  39 kB  00:00:00
(20/68): ksh-20120801-139.0.1.el7.x86_64.rpm                                                                                                               | 883 kB  00:00:00
(21/68): libX11-1.6.5-2.el7.x86_64.rpm                                                                                                                     | 606 kB  00:00:00
(22/68): libXau-1.0.8-2.1.el7.x86_64.rpm                                                                                                                   |  29 kB  00:00:00
(23/68): libX11-common-1.6.5-2.el7.noarch.rpm                                                                                                              | 163 kB  00:00:00
(24/68): libXinerama-1.1.3-2.1.el7.x86_64.rpm                                                                                                              |  14 kB  00:00:00
(25/68): libXext-1.3.3-3.el7.x86_64.rpm                                                                                                                    |  39 kB  00:00:00
(26/68): libXmu-1.1.2-2.el7.x86_64.rpm                                                                                                                     |  71 kB  00:00:00
(27/68): libXi-1.7.9-1.el7.x86_64.rpm                                                                                                                      |  40 kB  00:00:00
(28/68): libXrandr-1.5.1-2.el7.x86_64.rpm                                                                                                                  |  27 kB  00:00:00
(29/68): libXtst-1.2.3-1.el7.x86_64.rpm                                                                                                                    |  20 kB  00:00:00
(30/68): libXrender-0.9.10-1.el7.x86_64.rpm                                                                                                                |  26 kB  00:00:00
(31/68): libXv-1.0.11-1.el7.x86_64.rpm                                                                                                                     |  18 kB  00:00:00
(32/68): libXxf86dga-1.1.4-2.1.el7.x86_64.rpm                                                                                                              |  19 kB  00:00:00
(33/68): libXxf86misc-1.0.3-7.1.el7.x86_64.rpm                                                                                                             |  19 kB  00:00:00
(34/68): libXt-1.1.5-3.el7.x86_64.rpm                                                                                                                      | 173 kB  00:00:00
(35/68): libXxf86vm-1.1.4-1.el7.x86_64.rpm                                                                                                                 |  18 kB  00:00:00
(36/68): libbasicobjects-0.1.1-32.el7.x86_64.rpm                                                                                                           |  25 kB  00:00:00
(37/68): libaio-devel-0.3.109-13.el7.x86_64.rpm                                                                                                            |  13 kB  00:00:00
(38/68): libcollection-0.7.0-32.el7.x86_64.rpm                                                                                                             |  41 kB  00:00:00
(39/68): libevent-2.0.21-4.el7.x86_64.rpm                                                                                                                  | 214 kB  00:00:00
(40/68): libdmx-1.1.3-3.el7.x86_64.rpm                                                                                                                     |  16 kB  00:00:00
(41/68): libgcc-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                                | 101 kB  00:00:00
(42/68): libgomp-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                               | 157 kB  00:00:00
(43/68): libmpc-1.0.1-3.el7.x86_64.rpm                                                                                                                     |  51 kB  00:00:00
(44/68): libini_config-1.3.1-32.el7.x86_64.rpm                                                                                                             |  63 kB  00:00:00
(45/68): libpath_utils-0.2.1-32.el7.x86_64.rpm                                                                                                             |  28 kB  00:00:00
(46/68): libref_array-0.1.5-32.el7.x86_64.rpm                                                                                                              |  27 kB  00:00:00
(47/68): libnfsidmap-0.25-19.el7.x86_64.rpm                                                                                                                |  50 kB  00:00:00
(48/68): libstdc++-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                             | 304 kB  00:00:00
(49/68): libstdc++-devel-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                       | 1.5 MB  00:00:00
(50/68): libtirpc-0.2.4-0.15.el7.x86_64.rpm                                                                                                                |  88 kB  00:00:00
(51/68): libverto-libevent-0.2.5-4.el7.x86_64.rpm                                                                                                          | 8.9 kB  00:00:00
(52/68): libxcb-1.13-1.el7.x86_64.rpm                                                                                                                      | 213 kB  00:00:00
(53/68): lm_sensors-libs-3.4.0-6.20160601gitf9185e5.el7.x86_64.rpm                                                                                         |  41 kB  00:00:00
(54/68): mailx-12.5-19.el7.x86_64.rpm                                                                                                                      | 245 kB  00:00:00
(55/68): nfs-utils-1.3.0-0.61.0.1.el7.x86_64.rpm                                                                                                           | 410 kB  00:00:00
(56/68): oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64.rpm                                                                                         |  21 kB  00:00:00
(57/68): psmisc-22.20-15.el7.x86_64.rpm                                                                                                                    | 141 kB  00:00:00
(58/68): rpcbind-0.2.0-47.el7.x86_64.rpm                                                                                                                   |  59 kB  00:00:00
(59/68): quota-nls-4.01-17.el7.noarch.rpm                                                                                                                  |  90 kB  00:00:00
(60/68): quota-4.01-17.el7.x86_64.rpm                                                                                                                      | 179 kB  00:00:00
(61/68): smartmontools-6.5-1.el7.x86_64.rpm                                                                                                                | 460 kB  00:00:00
(62/68): mpfr-3.1.1-4.el7.x86_64.rpm                                                                                                                       | 203 kB  00:00:00
(63/68): sysstat-10.1.5-17.el7.x86_64.rpm                                                                                                                  | 314 kB  00:00:00
(64/68): xorg-x11-utils-7.5-23.el7.x86_64.rpm                                                                                                              | 114 kB  00:00:00
(65/68): unzip-6.0-19.el7.x86_64.rpm                                                                                                                       | 170 kB  00:00:00
(66/68): xorg-x11-xauth-1.0.9-1.el7.x86_64.rpm                                                                                                             |  30 kB  00:00:00
(67/68): tcp_wrappers-7.6-77.el7.x86_64.rpm                                                                                                                |  78 kB  00:00:00
(68/68): gcc-4.8.5-36.0.1.el7.x86_64.rpm                                                                                                                   |  16 MB  00:01:05
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                             973 kB/s |  64 MB  00:01:07
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
Importing GPG key 0xEC551F03:
 Userid     : "Oracle OSS group (Open Source Software group) <build@oss.oracle.com>"
 Fingerprint: 4214 4123 fecf c55b 9086 313d 72f9 7b74 ec55 1f03
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
Is this ok [y/N]: y
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : libgcc-4.8.5-36.0.1.el7.x86_64                                                                                                                                1/75
  Updating   : glibc-common-2.17-260.0.9.el7.x86_64                                                                                                                          2/75
  Updating   : glibc-2.17-260.0.9.el7.x86_64                                                                                                                                 3/75
  Updating   : libstdc++-4.8.5-36.0.1.el7.x86_64                                                                                                                             4/75
  Installing : mpfr-3.1.1-4.el7.x86_64                                                                                                                                       5/75
  Installing : libmpc-1.0.1-3.el7.x86_64                                                                                                                                     6/75
  Installing : libstdc++-devel-4.8.5-36.0.1.el7.x86_64                                                                                                                       7/75
  Installing : libref_array-0.1.5-32.el7.x86_64                                                                                                                              8/75
  Installing : libbasicobjects-0.1.1-32.el7.x86_64                                                                                                                           9/75
  Installing : libevent-2.0.21-4.el7.x86_64                                                                                                                                 10/75
  Installing : libICE-1.0.9-9.el7.x86_64                                                                                                                                    11/75
  Installing : libcollection-0.7.0-32.el7.x86_64                                                                                                                            12/75
  Installing : libXau-1.0.8-2.1.el7.x86_64                                                                                                                                  13/75
  Installing : libxcb-1.13-1.el7.x86_64                                                                                                                                     14/75
  Installing : libtirpc-0.2.4-0.15.el7.x86_64                                                                                                                               15/75
  Installing : rpcbind-0.2.0-47.el7.x86_64                                                                                                                                  16/75
  Updating   : 32:bind-license-9.9.4-72.el7.noarch                                                                                                                          17/75
  Installing : 32:bind-libs-9.9.4-72.el7.x86_64                                                                                                                             18/75
  Installing : 32:bind-utils-9.9.4-72.el7.x86_64                                                                                                                            19/75
  Installing : libSM-1.2.2-2.el7.x86_64                                                                                                                                     20/75
  Installing : libverto-libevent-0.2.5-4.el7.x86_64                                                                                                                         21/75
  Installing : cpp-4.8.5-36.0.1.el7.x86_64                                                                                                                                  22/75
  Installing : bc-1.06.95-13.el7.x86_64                                                                                                                                     23/75
  Installing : tcp_wrappers-7.6-77.el7.x86_64                                                                                                                               24/75
  Installing : psmisc-22.20-15.el7.x86_64                                                                                                                                   25/75
  Updating   : libgomp-4.8.5-36.0.1.el7.x86_64                                                                                                                              26/75
  Installing : libpath_utils-0.2.1-32.el7.x86_64                                                                                                                            27/75
  Installing : libini_config-1.3.1-32.el7.x86_64                                                                                                                            28/75
  Installing : gssproxy-0.7.0-21.el7.x86_64                                                                                                                                 29/75
  Installing : lm_sensors-libs-3.4.0-6.20160601gitf9185e5.el7.x86_64                                                                                                        30/75
  Installing : sysstat-10.1.5-17.el7.x86_64                                                                                                                                 31/75
  Installing : compat-libcap1-1.10-7.el7.x86_64                                                                                                                             32/75
  Installing : ksh-20120801-139.0.1.el7.x86_64                                                                                                                              33/75
  Installing : libnfsidmap-0.25-19.el7.x86_64                                                                                                                               34/75
  Installing : keyutils-1.5.8-3.el7.x86_64                                                                                                                                  35/75
  Installing : compat-libstdc++-33-3.2.3-72.el7.x86_64                                                                                                                      36/75
  Installing : unzip-6.0-19.el7.x86_64                                                                                                                                      37/75
  Installing : mailx-12.5-19.el7.x86_64                                                                                                                                     38/75
  Installing : 1:smartmontools-6.5-1.el7.x86_64                                                                                                                             39/75
  Installing : kernel-headers-3.10.0-957.el7.x86_64                                                                                                                         40/75
  Installing : glibc-headers-2.17-260.0.9.el7.x86_64                                                                                                                        41/75
  Installing : glibc-devel-2.17-260.0.9.el7.x86_64                                                                                                                          42/75
  Installing : gcc-4.8.5-36.0.1.el7.x86_64                                                                                                                                  43/75
  Installing : gcc-c++-4.8.5-36.0.1.el7.x86_64                                                                                                                              44/75
  Installing : libaio-devel-0.3.109-13.el7.x86_64                                                                                                                           45/75
  Installing : 1:quota-nls-4.01-17.el7.noarch                                                                                                                               46/75
  Installing : 1:quota-4.01-17.el7.x86_64                                                                                                                                   47/75
  Installing : 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64                                                                                                                        48/75
  Installing : libX11-common-1.6.5-2.el7.noarch                                                                                                                             49/75
  Installing : libX11-1.6.5-2.el7.x86_64                                                                                                                                    50/75
  Installing : libXext-1.3.3-3.el7.x86_64                                                                                                                                   51/75
  Installing : libXi-1.7.9-1.el7.x86_64                                                                                                                                     52/75
  Installing : libXrender-0.9.10-1.el7.x86_64                                                                                                                               53/75
  Installing : libXrandr-1.5.1-2.el7.x86_64                                                                                                                                 54/75
  Installing : libXtst-1.2.3-1.el7.x86_64                                                                                                                                   55/75
  Installing : libXxf86misc-1.0.3-7.1.el7.x86_64                                                                                                                            56/75
  Installing : libdmx-1.1.3-3.el7.x86_64                                                                                                                                    57/75
  Installing : libXinerama-1.1.3-2.1.el7.x86_64                                                                                                                             58/75
  Installing : libXxf86vm-1.1.4-1.el7.x86_64                                                                                                                                59/75
  Installing : libXv-1.0.11-1.el7.x86_64                                                                                                                                    60/75
  Installing : libXxf86dga-1.1.4-2.1.el7.x86_64                                                                                                                             61/75
  Installing : xorg-x11-utils-7.5-23.el7.x86_64                                                                                                                             62/75
  Installing : libXt-1.1.5-3.el7.x86_64                                                                                                                                     63/75
  Installing : libXmu-1.1.2-2.el7.x86_64                                                                                                                                    64/75
  Installing : 1:xorg-x11-xauth-1.0.9-1.el7.x86_64                                                                                                                          65/75
  Installing : kernel-container-3.10.0-0.0.0.2.el7.x86_64                                                                                                                   66/75
  Installing : oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64                                                                                                        67/75
  Updating   : 32:bind-libs-lite-9.9.4-72.el7.x86_64                                                                                                                        68/75
  Cleanup    : 32:bind-libs-lite-9.9.4-61.el7_5.1.x86_64                                                                                                                    69/75
  Cleanup    : libstdc++-4.8.5-28.el7_5.1.x86_64                                                                                                                            70/75
  Cleanup    : libgomp-4.8.5-28.el7_5.1.x86_64                                                                                                                              71/75
  Cleanup    : 32:bind-license-9.9.4-61.el7_5.1.noarch                                                                                                                      72/75
  Cleanup    : glibc-common-2.17-222.el7.x86_64                                                                                                                             73/75
  Cleanup    : glibc-2.17-222.el7.x86_64                                                                                                                                    74/75
  Cleanup    : libgcc-4.8.5-28.el7_5.1.x86_64                                                                                                                               75/75
  Verifying  : libXext-1.3.3-3.el7.x86_64                                                                                                                                    1/75
  Verifying  : libXxf86misc-1.0.3-7.1.el7.x86_64                                                                                                                             2/75
  Verifying  : libdmx-1.1.3-3.el7.x86_64                                                                                                                                     3/75
  Verifying  : libverto-libevent-0.2.5-4.el7.x86_64                                                                                                                          4/75
  Verifying  : libXinerama-1.1.3-2.1.el7.x86_64                                                                                                                              5/75
  Verifying  : libXrender-0.9.10-1.el7.x86_64                                                                                                                                6/75
  Verifying  : libref_array-0.1.5-32.el7.x86_64                                                                                                                              7/75
  Verifying  : glibc-headers-2.17-260.0.9.el7.x86_64                                                                                                                         8/75
  Verifying  : libXi-1.7.9-1.el7.x86_64                                                                                                                                      9/75
  Verifying  : libXxf86vm-1.1.4-1.el7.x86_64                                                                                                                                10/75
  Verifying  : glibc-devel-2.17-260.0.9.el7.x86_64                                                                                                                          11/75
  Verifying  : kernel-container-3.10.0-0.0.0.2.el7.x86_64                                                                                                                   12/75
  Verifying  : oracle-rdbms-server-12cR1-preinstall-1.0-7.el7.x86_64                                                                                                        13/75
  Verifying  : libgcc-4.8.5-36.0.1.el7.x86_64                                                                                                                               14/75
  Verifying  : 32:bind-libs-9.9.4-72.el7.x86_64                                                                                                                             15/75
  Verifying  : libX11-common-1.6.5-2.el7.noarch                                                                                                                             16/75
  Verifying  : sysstat-10.1.5-17.el7.x86_64                                                                                                                                 17/75
  Verifying  : bc-1.06.95-13.el7.x86_64                                                                                                                                     18/75
  Verifying  : libstdc++-4.8.5-36.0.1.el7.x86_64                                                                                                                            19/75
  Verifying  : libXv-1.0.11-1.el7.x86_64                                                                                                                                    20/75
  Verifying  : tcp_wrappers-7.6-77.el7.x86_64                                                                                                                               21/75
  Verifying  : xorg-x11-utils-7.5-23.el7.x86_64                                                                                                                             22/75
  Verifying  : glibc-common-2.17-260.0.9.el7.x86_64                                                                                                                         23/75
  Verifying  : gcc-4.8.5-36.0.1.el7.x86_64                                                                                                                                  24/75
  Verifying  : libXt-1.1.5-3.el7.x86_64                                                                                                                                     25/75
  Verifying  : 1:nfs-utils-1.3.0-0.61.0.1.el7.x86_64                                                                                                                        26/75
  Verifying  : libstdc++-devel-4.8.5-36.0.1.el7.x86_64                                                                                                                      27/75
  Verifying  : 32:bind-libs-lite-9.9.4-72.el7.x86_64                                                                                                                        28/75
  Verifying  : libbasicobjects-0.1.1-32.el7.x86_64                                                                                                                          29/75
  Verifying  : psmisc-22.20-15.el7.x86_64                                                                                                                                   30/75
  Verifying  : libini_config-1.3.1-32.el7.x86_64                                                                                                                            31/75
  Verifying  : libgomp-4.8.5-36.0.1.el7.x86_64                                                                                                                              32/75
  Verifying  : 1:quota-nls-4.01-17.el7.noarch                                                                                                                               33/75
  Verifying  : gcc-c++-4.8.5-36.0.1.el7.x86_64                                                                                                                              34/75
  Verifying  : 32:bind-utils-9.9.4-72.el7.x86_64                                                                                                                            35/75
  Verifying  : libpath_utils-0.2.1-32.el7.x86_64                                                                                                                            36/75
  Verifying  : lm_sensors-libs-3.4.0-6.20160601gitf9185e5.el7.x86_64                                                                                                        37/75
  Verifying  : libevent-2.0.21-4.el7.x86_64                                                                                                                                 38/75
  Verifying  : cpp-4.8.5-36.0.1.el7.x86_64                                                                                                                                  39/75
  Verifying  : libX11-1.6.5-2.el7.x86_64                                                                                                                                    40/75
  Verifying  : gssproxy-0.7.0-21.el7.x86_64                                                                                                                                 41/75
  Verifying  : rpcbind-0.2.0-47.el7.x86_64                                                                                                                                  42/75
  Verifying  : libxcb-1.13-1.el7.x86_64                                                                                                                                     43/75
  Verifying  : compat-libcap1-1.10-7.el7.x86_64                                                                                                                             44/75
  Verifying  : libXrandr-1.5.1-2.el7.x86_64                                                                                                                                 45/75
  Verifying  : libaio-devel-0.3.109-13.el7.x86_64                                                                                                                           46/75
  Verifying  : libmpc-1.0.1-3.el7.x86_64                                                                                                                                    47/75
  Verifying  : 1:quota-4.01-17.el7.x86_64                                                                                                                                   48/75
  Verifying  : 1:xorg-x11-xauth-1.0.9-1.el7.x86_64                                                                                                                          49/75
  Verifying  : 32:bind-license-9.9.4-72.el7.noarch                                                                                                                          50/75
  Verifying  : ksh-20120801-139.0.1.el7.x86_64                                                                                                                              51/75
  Verifying  : libICE-1.0.9-9.el7.x86_64                                                                                                                                    52/75
  Verifying  : glibc-2.17-260.0.9.el7.x86_64                                                                                                                                53/75
  Verifying  : libnfsidmap-0.25-19.el7.x86_64                                                                                                                               54/75
  Verifying  : kernel-headers-3.10.0-957.el7.x86_64                                                                                                                         55/75
  Verifying  : libSM-1.2.2-2.el7.x86_64                                                                                                                                     56/75
  Verifying  : 1:smartmontools-6.5-1.el7.x86_64                                                                                                                             57/75
  Verifying  : libXxf86dga-1.1.4-2.1.el7.x86_64                                                                                                                             58/75
  Verifying  : mpfr-3.1.1-4.el7.x86_64                                                                                                                                      59/75
  Verifying  : keyutils-1.5.8-3.el7.x86_64                                                                                                                                  60/75
  Verifying  : compat-libstdc++-33-3.2.3-72.el7.x86_64                                                                                                                      61/75
  Verifying  : libcollection-0.7.0-32.el7.x86_64                                                                                                                            62/75
  Verifying  : libXau-1.0.8-2.1.el7.x86_64                                                                                                                                  63/75
  Verifying  : libXtst-1.2.3-1.el7.x86_64                                                                                                                                   64/75
  Verifying  : unzip-6.0-19.el7.x86_64                                                                                                                                      65/75
  Verifying  : mailx-12.5-19.el7.x86_64                                                                                                                                     66/75
  Verifying  : libXmu-1.1.2-2.el7.x86_64                                                                                                                                    67/75
  Verifying  : libtirpc-0.2.4-0.15.el7.x86_64                                                                                                                               68/75
  Verifying  : 32:bind-libs-lite-9.9.4-61.el7_5.1.x86_64                                                                                                                    69/75
  Verifying  : libgomp-4.8.5-28.el7_5.1.x86_64                                                                                                                              70/75
  Verifying  : glibc-2.17-222.el7.x86_64                                                                                                                                    71/75
  Verifying  : 32:bind-license-9.9.4-61.el7_5.1.noarch                                                                                                                      72/75
  Verifying  : libgcc-4.8.5-28.el7_5.1.x86_64                                                                                                                               73/75
  Verifying  : glibc-common-2.17-222.el7.x86_64                                                                                                                             74/75
  Verifying  : libstdc++-4.8.5-28.el7_5.1.x86_64                                                                                                                            75/75

Installed:
  oracle-rdbms-server-12cR1-preinstall.x86_64 0:1.0-7.el7

Dependency Installed:
  bc.x86_64 0:1.06.95-13.el7                           bind-libs.x86_64 32:9.9.4-72.el7                                   bind-utils.x86_64 32:9.9.4-72.el7
  compat-libcap1.x86_64 0:1.10-7.el7                   compat-libstdc++-33.x86_64 0:3.2.3-72.el7                          cpp.x86_64 0:4.8.5-36.0.1.el7
  gcc.x86_64 0:4.8.5-36.0.1.el7                        gcc-c++.x86_64 0:4.8.5-36.0.1.el7                                  glibc-devel.x86_64 0:2.17-260.0.9.el7
  glibc-headers.x86_64 0:2.17-260.0.9.el7              gssproxy.x86_64 0:0.7.0-21.el7                                     kernel-container.x86_64 0:3.10.0-0.0.0.2.el7
  kernel-headers.x86_64 0:3.10.0-957.el7               keyutils.x86_64 0:1.5.8-3.el7                                      ksh.x86_64 0:20120801-139.0.1.el7
  libICE.x86_64 0:1.0.9-9.el7                          libSM.x86_64 0:1.2.2-2.el7                                         libX11.x86_64 0:1.6.5-2.el7
  libX11-common.noarch 0:1.6.5-2.el7                   libXau.x86_64 0:1.0.8-2.1.el7                                      libXext.x86_64 0:1.3.3-3.el7
  libXi.x86_64 0:1.7.9-1.el7                           libXinerama.x86_64 0:1.1.3-2.1.el7                                 libXmu.x86_64 0:1.1.2-2.el7
  libXrandr.x86_64 0:1.5.1-2.el7                       libXrender.x86_64 0:0.9.10-1.el7                                   libXt.x86_64 0:1.1.5-3.el7
  libXtst.x86_64 0:1.2.3-1.el7                         libXv.x86_64 0:1.0.11-1.el7                                        libXxf86dga.x86_64 0:1.1.4-2.1.el7
  libXxf86misc.x86_64 0:1.0.3-7.1.el7                  libXxf86vm.x86_64 0:1.1.4-1.el7                                    libaio-devel.x86_64 0:0.3.109-13.el7
  libbasicobjects.x86_64 0:0.1.1-32.el7                libcollection.x86_64 0:0.7.0-32.el7                                libdmx.x86_64 0:1.1.3-3.el7
  libevent.x86_64 0:2.0.21-4.el7                       libini_config.x86_64 0:1.3.1-32.el7                                libmpc.x86_64 0:1.0.1-3.el7
  libnfsidmap.x86_64 0:0.25-19.el7                     libpath_utils.x86_64 0:0.2.1-32.el7                                libref_array.x86_64 0:0.1.5-32.el7
  libstdc++-devel.x86_64 0:4.8.5-36.0.1.el7            libtirpc.x86_64 0:0.2.4-0.15.el7                                   libverto-libevent.x86_64 0:0.2.5-4.el7
  libxcb.x86_64 0:1.13-1.el7                           lm_sensors-libs.x86_64 0:3.4.0-6.20160601gitf9185e5.el7            mailx.x86_64 0:12.5-19.el7
  mpfr.x86_64 0:3.1.1-4.el7                            nfs-utils.x86_64 1:1.3.0-0.61.0.1.el7                              psmisc.x86_64 0:22.20-15.el7
  quota.x86_64 1:4.01-17.el7                           quota-nls.noarch 1:4.01-17.el7                                     rpcbind.x86_64 0:0.2.0-47.el7
  smartmontools.x86_64 1:6.5-1.el7                     sysstat.x86_64 0:10.1.5-17.el7                                     tcp_wrappers.x86_64 0:7.6-77.el7
  unzip.x86_64 0:6.0-19.el7                            xorg-x11-utils.x86_64 0:7.5-23.el7                                 xorg-x11-xauth.x86_64 1:1.0.9-1.el7

Dependency Updated:
  bind-libs-lite.x86_64 32:9.9.4-72.el7        bind-license.noarch 32:9.9.4-72.el7        glibc.x86_64 0:2.17-260.0.9.el7            glibc-common.x86_64 0:2.17-260.0.9.el7
  libgcc.x86_64 0:4.8.5-36.0.1.el7             libgomp.x86_64 0:4.8.5-36.0.1.el7          libstdc++.x86_64 0:4.8.5-36.0.1.el7

Complete!
```

</p>
</details>

安装完之后，会自动创建 oracle 用户和 oinstall 用户组。现在最好给 oracle 用户设置一下密码：
```shell
[root@localhost yum.repos.d]# passwd oracle
Changing password for user oracle.
New password:
BAD PASSWORD: The password is shorter than 8 characters
Retype new password:
passwd: all authentication tokens updated successfully.
```


## 前期准备
### 修改 hostname

修改默认主机名 `localhost.localdomain` 为 `oem.com`。若不修改，在后续安装 oracle 12c 的过程中，可能会出现 `oracle net configuration failed`，为了避免后续安装的复杂性，建议修改主机名。
先修改`/etc/hosts`为以下内容：
```shell
[root@localhost yum.repos.d]# vi /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.12.xxx oem oem.com
```
然后修改 `/etc/sysconfig/network` 为以下内容：
```shell
[root@localhost yum.repos.d]# vi /etc/sysconfig/network
# Created by anaconda
# oracle-rdbms-server-12cR1-preinstall : Add NOZEROCONF=yes
NOZEROCONF=yes
HOSTNAME=oem.com
```
最后重启电脑，并检查 hostname 是否修改成功：
```shell
[root@localhost yum.repos.d]# reboot
[root@oem ~] hostnamectl
   Static hostname: localhost.localdomain
Transient hostname: oem #看这里！！！表示修改成功！！！
         Icon name: computer-vm
           Chassis: vm
        Machine ID: f8d5c3cadxxxxxxxx3faaee11e76d665
           Boot ID: 9f4838b65fbe4dxxxxxxxxcb4ef8afad
    Virtualization: vmware
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-862.14.4.el7.x86_64
      Architecture: x86-64
```

### 配置 SSH 和 X11 转发
如果是在远程连接的情况下来安装 EM（比如使用VNC服务等），Oracle Universal Installer (OUI) GUI 需要使用X11会话来运行。所以首先需要配置SSH来支持X11转发（默认就是开启的，只需要确认一下即可）：
```shell
[root@oem ~]# vi /etc/ssh/sshd_config
#X11Forwarding no
X11Forwarding yes
#X11DisplayOffset 10
#X11UseLocalhost yes
[root@localhost ~]#
```

还需要安装 `xauth` 和 `xorg-x11-apps` 包：
```shell
[root@oem ~]# rpm -qa | grep -i xauth
xorg-x11-xauth-1.0.2-7.1.el6.x86_64
[root@oem ~]# rpm -qa | grep -i xorg-x11-apps
[root@oem ~]# yum -y install xorg-x11-apps xauth
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.njupt.edu.cn
 * extras: mirrors.aliyun.com
 * updates: mirrors.njupt.edu.cn
Package 1:xorg-x11-xauth-1.0.9-1.el7.x86_64 already installed and latest version
Resolving Dependencies
--> Running transaction check
---> Package xorg-x11-apps.x86_64 0:7.7-7.el7 will be installed
--> Processing Dependency: libpng15.so.15(PNG15_0)(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libxkbfile.so.1()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libpng15.so.15()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libfontenc.so.1()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libfontconfig.so.1()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libXft.so.2()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libXcursor.so.1()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Processing Dependency: libXaw.so.7()(64bit) for package: xorg-x11-apps-7.7-7.el7.x86_64
--> Running transaction check
---> Package fontconfig.x86_64 0:2.13.0-4.3.el7 will be installed
--> Processing Dependency: freetype >= 2.8-7 for package: fontconfig-2.13.0-4.3.el7.x86_64
--> Processing Dependency: fontpackages-filesystem for package: fontconfig-2.13.0-4.3.el7.x86_64
--> Processing Dependency: dejavu-sans-fonts for package: fontconfig-2.13.0-4.3.el7.x86_64
---> Package libXaw.x86_64 0:1.0.13-4.el7 will be installed
--> Processing Dependency: libXpm.so.4()(64bit) for package: libXaw-1.0.13-4.el7.x86_64
---> Package libXcursor.x86_64 0:1.1.15-1.el7 will be installed
--> Processing Dependency: libXfixes.so.3()(64bit) for package: libXcursor-1.1.15-1.el7.x86_64
---> Package libXft.x86_64 0:2.3.2-2.el7 will be installed
---> Package libfontenc.x86_64 0:1.1.3-3.el7 will be installed
---> Package libpng.x86_64 2:1.5.13-7.el7_2 will be installed
---> Package libxkbfile.x86_64 0:1.0.9-3.el7 will be installed
--> Running transaction check
---> Package dejavu-sans-fonts.noarch 0:2.33-6.el7 will be installed
--> Processing Dependency: dejavu-fonts-common = 2.33-6.el7 for package: dejavu-sans-fonts-2.33-6.el7.noarch
---> Package fontpackages-filesystem.noarch 0:1.44-8.el7 will be installed
---> Package freetype.x86_64 0:2.4.11-15.el7 will be updated
---> Package freetype.x86_64 0:2.8-12.el7 will be an update
---> Package libXfixes.x86_64 0:5.0.3-1.el7 will be installed
---> Package libXpm.x86_64 0:3.5.12-1.el7 will be installed
--> Running transaction check
---> Package dejavu-fonts-common.noarch 0:2.33-6.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==================================================================================================================================================================================
 Package                                             Arch                               Version                                      Repository                              Size
==================================================================================================================================================================================
Installing:
 xorg-x11-apps                                       x86_64                             7.7-7.el7                                    base                                   307 k
Installing for dependencies:
 dejavu-fonts-common                                 noarch                             2.33-6.el7                                   base                                    64 k
 dejavu-sans-fonts                                   noarch                             2.33-6.el7                                   base                                   1.4 M
 fontconfig                                          x86_64                             2.13.0-4.3.el7                               ol7_latest                             254 k
 fontpackages-filesystem                             noarch                             1.44-8.el7                                   base                                   9.9 k
 libXaw                                              x86_64                             1.0.13-4.el7                                 base                                   192 k
 libXcursor                                          x86_64                             1.1.15-1.el7                                 ol7_latest                              30 k
 libXfixes                                           x86_64                             5.0.3-1.el7                                  base                                    18 k
 libXft                                              x86_64                             2.3.2-2.el7                                  base                                    58 k
 libXpm                                              x86_64                             3.5.12-1.el7                                 base                                    55 k
 libfontenc                                          x86_64                             1.1.3-3.el7                                  base                                    31 k
 libpng                                              x86_64                             2:1.5.13-7.el7_2                             base                                   213 k
 libxkbfile                                          x86_64                             1.0.9-3.el7                                  base                                    83 k
Updating for dependencies:
 freetype                                            x86_64                             2.8-12.el7                                   ol7_latest                             379 k

Transaction Summary
==================================================================================================================================================================================
Install  1 Package  (+12 Dependent packages)
Upgrade             (  1 Dependent package)

Total download size: 3.1 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/14): dejavu-fonts-common-2.33-6.el7.noarch.rpm                                                                                                          |  64 kB  00:00:00
(2/14): libXaw-1.0.13-4.el7.x86_64.rpm                                                                                                                     | 192 kB  00:00:00
(3/14): fontpackages-filesystem-1.44-8.el7.noarch.rpm                                                                                                      | 9.9 kB  00:00:01
(4/14): freetype-2.8-12.el7.x86_64.rpm                                                                                                                     | 379 kB  00:00:01
(5/14): fontconfig-2.13.0-4.3.el7.x86_64.rpm                                                                                                               | 254 kB  00:00:02
(6/14): libXfixes-5.0.3-1.el7.x86_64.rpm                                                                                                                   |  18 kB  00:00:00
(7/14): libXpm-3.5.12-1.el7.x86_64.rpm                                                                                                                     |  55 kB  00:00:00
(8/14): libXft-2.3.2-2.el7.x86_64.rpm                                                                                                                      |  58 kB  00:00:00
(9/14): libfontenc-1.1.3-3.el7.x86_64.rpm                                                                                                                  |  31 kB  00:00:00
(10/14): libpng-1.5.13-7.el7_2.x86_64.rpm                                                                                                                  | 213 kB  00:00:00
(11/14): libxkbfile-1.0.9-3.el7.x86_64.rpm                                                                                                                 |  83 kB  00:00:00
(12/14): libXcursor-1.1.15-1.el7.x86_64.rpm                                                                                                                |  30 kB  00:00:00
(13/14): dejavu-sans-fonts-2.33-6.el7.noarch.rpm                                                                                                           | 1.4 MB  00:00:03
(14/14): xorg-x11-apps-7.7-7.el7.x86_64.rpm                                                                                                                | 307 kB  00:00:00
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                             965 kB/s | 3.1 MB  00:00:03
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : fontpackages-filesystem-1.44-8.el7.noarch                                                                                                                     1/15
  Installing : 2:libpng-1.5.13-7.el7_2.x86_64                                                                                                                                2/15
  Updating   : freetype-2.8-12.el7.x86_64                                                                                                                                    3/15
  Installing : dejavu-fonts-common-2.33-6.el7.noarch                                                                                                                         4/15
  Installing : dejavu-sans-fonts-2.33-6.el7.noarch                                                                                                                           5/15
  Installing : fontconfig-2.13.0-4.3.el7.x86_64                                                                                                                              6/15
  Installing : libXft-2.3.2-2.el7.x86_64                                                                                                                                     7/15
  Installing : libfontenc-1.1.3-3.el7.x86_64                                                                                                                                 8/15
  Installing : libXfixes-5.0.3-1.el7.x86_64                                                                                                                                  9/15
  Installing : libXcursor-1.1.15-1.el7.x86_64                                                                                                                               10/15
  Installing : libxkbfile-1.0.9-3.el7.x86_64                                                                                                                                11/15
  Installing : libXpm-3.5.12-1.el7.x86_64                                                                                                                                   12/15
  Installing : libXaw-1.0.13-4.el7.x86_64                                                                                                                                   13/15
  Installing : xorg-x11-apps-7.7-7.el7.x86_64                                                                                                                               14/15
  Cleanup    : freetype-2.4.11-15.el7.x86_64                                                                                                                                15/15
  Verifying  : libXpm-3.5.12-1.el7.x86_64                                                                                                                                    1/15
  Verifying  : fontconfig-2.13.0-4.3.el7.x86_64                                                                                                                              2/15
  Verifying  : libxkbfile-1.0.9-3.el7.x86_64                                                                                                                                 3/15
  Verifying  : libXaw-1.0.13-4.el7.x86_64                                                                                                                                    4/15
  Verifying  : libXfixes-5.0.3-1.el7.x86_64                                                                                                                                  5/15
  Verifying  : dejavu-fonts-common-2.33-6.el7.noarch                                                                                                                         6/15
  Verifying  : dejavu-sans-fonts-2.33-6.el7.noarch                                                                                                                           7/15
  Verifying  : libXcursor-1.1.15-1.el7.x86_64                                                                                                                                8/15
  Verifying  : 2:libpng-1.5.13-7.el7_2.x86_64                                                                                                                                9/15
  Verifying  : libXft-2.3.2-2.el7.x86_64                                                                                                                                    10/15
  Verifying  : libfontenc-1.1.3-3.el7.x86_64                                                                                                                                11/15
  Verifying  : freetype-2.8-12.el7.x86_64                                                                                                                                   12/15
  Verifying  : fontpackages-filesystem-1.44-8.el7.noarch                                                                                                                    13/15
  Verifying  : xorg-x11-apps-7.7-7.el7.x86_64                                                                                                                               14/15
  Verifying  : freetype-2.4.11-15.el7.x86_64                                                                                                                                15/15

Installed:
  xorg-x11-apps.x86_64 0:7.7-7.el7

Dependency Installed:
  dejavu-fonts-common.noarch 0:2.33-6.el7      dejavu-sans-fonts.noarch 0:2.33-6.el7      fontconfig.x86_64 0:2.13.0-4.3.el7      fontpackages-filesystem.noarch 0:1.44-8.el7
  libXaw.x86_64 0:1.0.13-4.el7                 libXcursor.x86_64 0:1.1.15-1.el7           libXfixes.x86_64 0:5.0.3-1.el7          libXft.x86_64 0:2.3.2-2.el7
  libXpm.x86_64 0:3.5.12-1.el7                 libfontenc.x86_64 0:1.1.3-3.el7            libpng.x86_64 2:1.5.13-7.el7_2          libxkbfile.x86_64 0:1.0.9-3.el7

Dependency Updated:
  freetype.x86_64 0:2.8-12.el7

Complete!
```

为保险起见，此时最好重启系统。

## 安装 Oracle 12c
本章开始在 Centos7 上安装 Oracle Database 12c Release 1（12.1.0.2.0）

### 创建目录和部署安装文件
Oracle 12c 可以从[这里](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html)下载到。

以 root 用户创建保存安装文件的目录：
```shell
[root@oem ~]# mkdir -p /u01/stage
[root@oem ~]# chown -R oracle:oinstall /u01/*
```
切换回 oracle 用户，创建目录：
```shell
[oracle@oem ~]$ mkdir /u01/stage/db12c
```
将下载好的数据库安装文件拷贝到 `/u01/stage/db12c` 目录，并解压：
```shell
[oracle@oem ~]$ cd /u01/stage/db12c/
[oracle@oem db12c]$ ll
total 2625080
-rw-r--r--. 1 oracle oinstall 1673544724 Nov 12 15:38 linuxamd64_12102_database_1of2.zip
-rw-r--r--. 1 oracle oinstall 1014530602 Nov 12 15:38 linuxamd64_12102_database_2of2.zip
[oracle@localhost db12c]$ unzip linuxamd64_12102_database_1of2.zip
[oracle@localhost db12c]$ unzip linuxamd64_12102_database_2of2.zip
[oracle@localhost db12c]$ ll
total 2625080
drwxr-xr-x. 7 oracle oinstall        117 Jul  7  2014 database
-rw-r--r--. 1 oracle oinstall 1673544724 Nov 12 15:38 linuxamd64_12102_database_1of2.zip
-rw-r--r--. 1 oracle oinstall 1014530602 Nov 12 15:38 linuxamd64_12102_database_2of2.zip
```

### 创建安装用目录（按照OFA标准）

[Optimal Flexible Architecture (OFA)](https://en.wikipedia.org/wiki/Optimal_Flexible_Architecture) 标准是为管理Oracle安装而定义的一套目录推荐命名标准。OFA提供了与Oracle Universal Installer相一致的挂载点，目录，文件名的命名规范。

以root用户执行以下命令来创建所需的各个目录：
```shell
[root@oem ~]# mkdir -p /u01/app/oracle/product/12.1.0/dbhome_1
[root@oem ~]# chown -R oracle:oinstall /u01/*
[root@oem ~]# chmod -R 775 /u01/*
```
注意： `/u01`这个目录的拥有者应该是 `root`。

### 修改 ulimit 值：最大文件描述符数为4096

安装完 `oracle-rdbms-server-12cR1-preinstall` 之后，会在 `/etc/security/limits.d` 自动生成配置文件 `oracle-rdbms-server-12cR1-preinstall.conf`。以root用户修改里面的值如下：
```shell
[root@oem zodas]# cd /etc/security/limits.d
[root@oem limits.d]# vi oracle-rdbms-server-12cR1-preinstall.conf 
#oracle   soft   nofile    1024
oracle   soft   nofile    4096
```
修改完之后，切换到oracle用户，查看ulimit值是否生效：
```shell
[root@oem limits.d]# su - oracle
[oracle@oem ~]$ ulimit -n
4096
[oracle@oem ~]$
```

### 修改 ulimit 值：最大用户进程数为16384
安装完 `oracle-rdbms-server-12cR1-preinstall`之后，会在`/etc/security/limits.d`自动生成配置文件`20-nproc.conf`。以root用户修改里面的值如下：
```shell
[root@oem oracle]# cd /etc/security/limits.d
[root@oem limits.d]# vi 20-nproc.conf
# Default limit for number of user's processes to prevent
# accidental fork bombs.
# See rhbz #432903 for reasoning.

#*          soft    nproc     4096
*          -    nproc     16384
root       soft    nproc     unlimited
```

切换为 oracle 用户，查看修改结果：
```shell
[oracle@oem ~]$ ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 7191
max locked memory       (kbytes, -l) 134217728
max memory size         (kbytes, -m) unlimited
open files                      (-n) 4096  ##看这里！！！
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 10240
cpu time               (seconds, -t) unlimited
max user processes              (-u) 16384 ##看这里！！！
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

### 增大 tmpfs 到4GB

默认的 tmpfs 的值过小，Oracle数据库启动时可能会报错（ORA-00838，ORA-00845）。为了防止这种错误，先增大 tmpfs 的值到4GB。
```shell
[root@oem zodas]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   35G  9.2G   26G  27% /
devtmpfs                 1.9G     0  1.9G   0% /dev
tmpfs                    1.9G     0  1.9G   0% /dev/shm
tmpfs                    1.9G   12M  1.9G   1% /run
tmpfs                    1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda2               1014M  184M  831M  19% /boot
/dev/mapper/centos-home   30G   33M   30G   1% /home
tmpfs                    378M     0  378M   0% /run/user/1000
[root@oem oracle]# mount -t tmpfs shmfs -o size=4000m /dev/shm
[root@oem oracle]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   35G  9.2G   26G  27% /
devtmpfs                 1.9G     0  1.9G   0% /dev
shmfs                    4.0G     0  4.0G   0% /dev/shm
tmpfs                    1.9G   12M  1.9G   1% /run
tmpfs                    1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda2               1014M  184M  831M  19% /boot
/dev/mapper/centos-home   30G   33M   30G   1% /home
tmpfs                    378M     0  378M   0% /run/user/1000
```
同时修改 `/etc/fstab` 文件，使配置永久生效：
```shell
[root@oem zodas]# vi /etc/fstab
#
# /etc/fstab
# Created by anaconda on Mon Nov 12 00:31:16 2018
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=9ffb8583-88fa-4fbe-8cb5-9c21c8fbfa9c /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
tmpfs			/dev/shm		tmpfs	size=4000m	0 0 ##看这里！！！
```

重启系统后，应该能看到如下的内容：
```shell
[zodas@oem ~]$ df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   35G  9.3G   26G  27% /
devtmpfs                 1.9G     0  1.9G   0% /dev
tmpfs                    4.0G     0  4.0G   0% /dev/shm
tmpfs                    1.9G   12M  1.9G   1% /run
tmpfs                    1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/mapper/centos-home   30G   33M   30G   1% /home
/dev/sda2               1014M  172M  843M  17% /boot
tmpfs                    378M     0  378M   0% /run/user/1000
```

### 若 swap space < 4G，则需要扩充 swap space

使用如下命令查看 swap space:

```shell
[root@oem ~]# swapon -s
Filename				Type		Size	Used	Priority
/dev/dm-1                              	partition	2097148	0	-1
```

执行以下命令，增加 swap space
```shell
[root@oem ~]# dd if=/dev/zero of=/swapfile bs=1M count=4096
4096+0 records in
4096+0 records out
4294967296 bytes (4.3 GB) copied, 25.9202 s, 166 MB/s
[root@oem ~]# mkswap /swapfile
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=fcdf3cfe-9352-4d77-b4dc-3517b41cb18a
[root@localhost ~]# swapon /swapfile
swapon: /swapfile: insecure permissions 0644, 0600 suggested.
[root@localhost db12c]# swapon -s
Filename				Type		Size	Used	Priority
/dev/dm-1                              	partition	2097148	0	-1
/swapfile                              	file	4194300	0	-2
```

### 配置环境变量 /home/oracle/.bashrc
以 oracle 用户，在 `~/.bashrc` 的末尾加入如下配置内容：
```shell
# Oracle variables
ORACLE_HOSTNAME=oem.com; export ORACLE_HOSTNAME
ORACLE_BASE=/u01/app/oracle; export ORACLE_BASE
ORACLE_HOME=$ORACLE_BASE/product/12.1.0/dbhome_1; export ORACLE_HOME
AGENT_HOME=$ORACLE_BASE/product/agentr4/agent_inst; export AGENT_HOME
OMS_HOME=$ORACLE_BASE/product/MiddlewareR4/oms; export OMS_HOME
ORACLE_INSTANCE=/u01/app/oracle/product/gc_inst/WebTierIH1; export ORACLE_INSTANCE
ORACLE_SID=omr; export ORACLE_SID
ORACLE_UNQNAME=omr; export ORACLE_UNQNAME
PATH=$ORACLE_HOME/bin:$PATH; export PATH
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib; export LD_LIBRARY_PATH
```
并使之生效：
```shell
[oracle@oem ~]$ source .bashrc
```

## 开始安装 oracle 数据库

打开一个新的终端，远程连接服务器GUI界面安装 oracle：
```shell
[mac]ssh -Y oracle@192.168.12.xxx
oracle@192.168.12.xxx's password:
Last login: Mon Nov 12 16:30:05 2018
/usr/bin/xauth:  file /home/oracle/.Xauthority does not exist
[oracle@oem ~]$ /u01/stage/db12c/database/runInstaller
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 500 MB.   Actual 5597 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 2047 MB    Passed
Checking monitor: must be configured to display at least 256 colors.    Actual 16777216    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2018-11-12_04-35-35PM. Please wait ...[oracle@localhost database]$
```

> 在运行 `runInstaller` 时，可能会出现 `Checking monitor: must be configured to display at least 256 colors. Failed`。对于 macOS，安装 `xquartz`方可解决；或者让服务器(你所要安装 oracle 的那台机器）以图形化模式运行而不是命令行模式，然后直接在服务器上（而不是ssh 远程连接服务器的行驶）执行`/u01/stage/db12c/database/runInstaller`即可。

打开 mac 的 terminal，用 homebrew 安装 `xquartz`：
```shell
[mac]$ brew cask install xquartz
```

说明：
- *参考我的另一篇[博文](http://seyvoue.com/posts/36d45381/)，了解 homebrew 的安装与使用*
- *参考[此文](https://medium.com/@toja/using-x11-apps-in-mac-os-x-c74b304fd128)，了解如何能在 macos 上显示 x11 apps*
- *参考[此文](http://www.ipaomi.com/2017/11/09/%E8%BF%9C%E7%A8%8B%E6%98%BE%E7%A4%BA%E6%93%8D%E4%BD%9C-%E6%9C%8D%E5%8A%A1%E5%99%A8-gui-%E7%A8%8B%E5%BA%8F%E5%9B%BE%E5%BD%A2%E5%8C%96%E7%95%8C%E9%9D%A2-%E5%9F%BA%E4%BA%8E-x11-forwarding-centos/)，了解什么是 X Window System及其工作原理*

### Configure Security Updates

不选择 `I wish to receive security updates via My Oracle Support`，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-084956.png)

### Select Installation Option

勾选 `Create and configure a database`，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085050.png)


### Server Class

勾选 `Server Class`，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085212.png)

### Grid Installation Options

勾选 `Single instance database installation`，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085328.png)
### Select Install Type

勾选 `Advanced install`，点击 `Nex`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085353.png)
### Select Product Languages

如果要支持多语言的话，勾选对应的语言，本文加入了 `Simplified Chinese` 支持，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085452.png)
### Select Database Edition

保持默认选择（`Enterprise Edition (6.4GB)`），直接点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085511.png)
### Specify Installation Location

保持默认选择:
- Oracle base: `/u01/app/oracle`
- Software location: `/u01/app/oracle/product/12.1.0/dbhome_1`
直接点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085656.png)

### Create Inventory

保持默认选择:
- Inventory Directory: `/u01/app/oraInventory`
- oraInventory Group Name: `oinstall`
直接点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085853.png)

### Select Configuration Type

保持默认选择（`General Purpose / Transaction Processing`），直接点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-085932.png)

### Specify Database Identifiers
设置：
- Global database name: `omr.com`
- Oracle system identifier (SID): `omr`
同时一定不要勾选 `Create as Container database`。点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-181603.png)

### Specify Configuration Options

由于是测试环境的内存容量有限，所以将内存先设置为 `1024`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-091658.png)

字符集选择可以支持任何语言的`Use Unicode (AL32UTF8)`。点击 `Next`
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-091721.png)

### Specify Database Storage Options

保持默认选择（`File System`）。点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-091814.png)

### Specify Management Options

保持默认选择（先不注册到EM中）。点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-091838.png)

### Specify Recovery Options

保持默认选择（先不启用数据库恢复）。点击 `Next`
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-091908.png)

### Specify Schema Passwords

选择 `Use the same password for all accounts`，并设置密码，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-092047.png)

### Specify Operating System groups

全部都选择 `dba`，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-092122.png)

### Summary

最后检查一遍配置，没有问题的话，点击 `Install`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-092630.png)

### Install Product

开始漫长的安装过程，请耐心等待。。。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-095259.png)

### Execute Configuration scripts
这时需要以root用户执行两个脚本：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-204249.png)
打开一个终端，切换到root用户，执行以下命令：
```shell
[oracle@oem ~]$ su -
Password: 
[root@oem zodas]# /u01/app/oraInventory/orainstRoot.sh
Changing permissions of /u01/app/oraInventory.
Adding read,write permissions for group.
Removing read,write,execute permissions for world.

Changing groupname of /u01/app/oraInventory to oinstall.
The execution of the script is complete.
[root@oem zodas]# /u01/app/oracle/product/12.1.0/dbhome_1/root.sh
Performing root user operation.

The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /u01/app/oracle/product/12.1.0/dbhome_1

Enter the full pathname of the local bin directory: [/usr/local/bin]:
   Copying dbhome to /usr/local/bin ...
   Copying oraenv to /usr/local/bin ...
   Copying coraenv to /usr/local/bin ...


Creating /etc/oratab file...
Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root script.
Now product-specific root actions will be performed.
```
#### 可能出现的问题
若完全按照本文一步一步操作，一般来说是不会有问题的。如果，在**前期准备**时，没有修改 hostname，只是简单的修改了 `/etc/hosts`文件，如按照[该文](http://xintq.net/2015/08/04/oracle-enterprise-manager-12c-installation/#install-product)，执行完脚本后，可能会出现 `Oracle Net Configuration Assistant failed`:
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-181958.png)

先选择 `Skip`，以让安装能够继续进行（后续如果修复这个问题，请参考我的另一篇博文[“@解决 Oracle Net Configuration Assistant Failed 问题”](http://seyvoue.com/posts/428b54b1/#more)），安装完成后，得到的结果如下图：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-Screen%20Shot%202018-11-12%20at%2022.57.27.png)

### Database Configuration Assistant
看到下面的画面时，说明数据库已经安装完成。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-203348.png)

### Finish
这时可以关闭安装向导。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-12-171613.png)

顺便说一下，此时如果想删除数据库的话，可以执行`$ORACLE_HOME/deinstall/deinstall`脚本，简单一个命令就可以搞定了。

## 数据库安装后设置
### 数据库参数修改和监听服务查看
以root用户修改/etc/oratab文件，将restart标志位设置为Y:
```shell
[root@oem zodas]# vi /etc/oratab
# This file is used by ORACLE utilities.  It is created by root.sh
# and updated by either Database Configuration Assistant while creating
# a database or ASM Configuration Assistant while creating ASM instance.

# A colon, ':', is used as the field terminator.  A new line terminates
# the entry.  Lines beginning with a pound sign, '#', are comments.
#
# Entries are of the form:
#   $ORACLE_SID:$ORACLE_HOME:<N|Y>:
#
# The first and second fields are the system identifier and home
# directory of the database respectively.  The third field indicates
# to the dbstart utility that the database should , "Y", or should not,
# "N", be brought up at system boot time.
#
# Multiple entries with the same $ORACLE_SID are not allowed.
#
#
#omr:/u01/app/oracle/product/12.1.0/dbhome_1:N
omr:/u01/app/oracle/product/12.1.0/dbhome_1:Y
```

以oracle用户，创建Oracle数据库的`redo`日志文件夹：
```shell
[oracle@oem ~]$ mkdir /u01/app/oracle/product/redo_logs/
```
接着，登录到数据库中，先从spfile创建一个pfile：
```shell
[oracle@oem ~]$ sqlplus / as sysdba

SQL*Plus: Release 12.1.0.2.0 Production on Tue Nov 13 01:22:41 2018

Copyright (c) 1982, 2014, Oracle.  All rights reserved.


Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL> create pfile from spfile;

File created.

```
然后，修改数据库参数并创建redo文件：
```shell
SQL> ALTER SYSTEM SET processes=300 SCOPE=SPFILE;

System altered.

SQL> ALTER SYSTEM SET session_cached_cursors=200 SCOPE=SPFILE;

System altered.

SQL> ALTER SYSTEM SET db_securefile=PERMITTED SCOPE=BOTH;

System altered.

SQL> ALTER DATABASE ADD LOGFILE GROUP 4 ('/u01/app/oracle/product/redo_logs/log1a.rdo') SIZE 300M REUSE;

Database altered.

SQL> ALTER DATABASE ADD LOGFILE GROUP 5 ('/u01/app/oracle/product/redo_logs/log2a.rdo') SIZE 300M REUSE;

Database altered.

SQL> ALTER DATABASE ADD LOGFILE GROUP 6 ('/u01/app/oracle/product/redo_logs/log3a.rdo') SIZE 300M REUSE;

Database altered.
```
重启数据库，使配置生效：
```shell
SQL> SHUTDOWN IMMEDIATE;
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> STARTUP;
ORACLE instance started.

Total System Global Area 1073741824 bytes
Fixed Size		    2932632 bytes
Variable Size		  713031784 bytes
Database Buffers	  352321536 bytes
Redo Buffers		    5455872 bytes
Database mounted.
Database opened.
```
确认一下HTTPS的端口是否是5500，并退出sqlplus：
```shell
SQL> SELECT dbms_xdb_config.gethttpsport FROM dual;

GETHTTPSPORT
------------
	5500
SQL> EXIT
Disconnected from Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options
```
最后确认LISTNER的状态，看看监听服务是否正常：
```shell
[oracle@oem ~]$ lsnrctl status

LSNRCTL for Linux: Version 12.1.0.2.0 - Production on 13-NOV-2018 04:54:19

Copyright (c) 1991, 2014, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=oem.com)(PORT=1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
Start Date                13-NOV-2018 04:26:03
Uptime                    0 days 0 hr. 28 min. 16 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/12.1.0/dbhome_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/oem/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=oem)(PORT=1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcps)(HOST=oem)(PORT=5500))(Security=(my_wallet_directory=/u01/app/oracle/admin/omr/xdb_wallet))(Presentation=HTTP)(Session=RAW))
Services Summary...
Service "omr.com" has 1 instance(s).
  Instance "omr", status READY, has 1 handler(s) for this service...
Service "omrXDB.com" has 1 instance(s).
  Instance "omr", status READY, has 1 handler(s) for this service...
The command completed successfully
```
可以看到数据库服务`omr.example.com`已经就绪。

### 开机自动启动数据库（可选）
其实到上一步骤为止，Oracle的数据库安装已经完成。
如果还需要在开机时自动启动数据服务的话，可以按照下面的步骤实现。

首先以root用户创建一个自启动脚本：
```shell
[root@oem ~]# vim /etc/init.d/dbora
#!/bin/sh
# chkconfig: 345 99 10
# description: Oracle auto start-stop script.
#
# Set ORA_HOME to be equivalent to the $ORACLE_HOME
# from which you wish to execute dbstart and dbshut;
#
# Set ORA_OWNER to the user id of the owner of the
# Oracle database in ORA_HOME.
ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
ORA_OWNER=oracle
if [ ! -f ${ORA_HOME}/bin/dbstart ]
then
        echo "Oracle startup: cannot start"
        exit
fi
case "$1" in
'start')
    # Start the Oracle databases:
    # The following command assumes that the oracle login
    # will not prompt the user for any values
    su - ${ORA_OWNER} -c "${ORA_HOME}/bin/dbstart $ORA_HOME"
    touch /var/lock/subsys/dbora
    ;;
'stop')
    # Stop the Oracle databases:
    # The following command assumes that the oracle login
    # will not prompt the user for any values
    su - ${ORA_OWNER} -c "${ORA_HOME}/bin/dbshut $ORA_HOME"
    rm -f /var/lock/subsys/dbora
    ;;
esac
```
以root用户修改脚本权限，并加入到开机启动服务列表中：
```shell
[root@oem ~]# chmod 750 /etc/init.d/dbora 
[root@oem ~]# chkconfig dbora on
[root@oem ~]# chkconfig --list|grep dbora
dbora          	0:off	1:off	2:on	3:on	4:on	5:on	6:off
```
最后，以root用户创建一些软链接，将自启动脚本加入到Oracle Linux 的启动进程中：
```shell
[root@oem ~]# ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
[root@oem ~]# ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S96dbora
[root@oem ~]# ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S96dbora
```
这时可以重启一下系统，验证一下数据库服务有没有随系统自动启动。

## 安装Oracle Enterprise Manager 12c R4
本章开始利用Oracle Universal Installer (OUI)在 centos 7上安装如下组件：
- Oracle Enterprise Manager 12c Release 4 (12.1.0.4)
- Oracle Management Service
- Oracle Management Agent
安装文件可以从[这里](https://www.oracle.com/technetwork/oem/grid-control/downloads/linuxx8664soft-085949.html)下载到。

### 安装前检查
以root用户先查看一下必须的软件包是否已经安装完成：
```shell
[oracle@oem ~]$ yum list make binutils gcc libaio glib-common libstdc++ libXtst systat glibc-devel glibc libaio glibc-devel.i686
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: ftp.sjtu.edu.cn
 * extras: ftp.sjtu.edu.cn
 * updates: ftp.sjtu.edu.cn
ol7_UEKR4                                                                       124/124
ol7_latest                                                                  11482/11482
Installed Packages
binutils.x86_64                      2.27-28.base.el7_5.1                    @updates
gcc.x86_64                           4.8.5-36.0.1.el7                        @ol7_latest
glibc.x86_64                         2.17-260.0.9.el7                        @ol7_latest
glibc-devel.x86_64                   2.17-260.0.9.el7                        @ol7_latest
libXtst.x86_64                       1.2.3-1.el7                             @base
libaio.x86_64                        0.3.109-13.el7                          @anaconda
libstdc++.x86_64                     4.8.5-36.0.1.el7                        @ol7_latest
make.x86_64                          1:3.82-23.el7                           @anaconda
Available Packages
binutils.x86_64                      2.27-34.base.0.1.el7                    ol7_latest
glibc.i686                           2.17-260.0.9.el7                        ol7_latest
glibc-devel.i686                     2.17-260.0.9.el7                        ol7_latest
libXtst.i686                         1.2.3-1.el7                             base
libaio.i686                          0.3.109-13.el7                          base
libstdc++.i686                       4.8.5-36.0.1.el7                        ol7_latest
```
安装上面 `Available Packages` 中列出的所有必须软件包：
```shell
[root@oem oracle]# yum install binutils.x86_64 glibc.i686 glibc-devel.i686 libXtst.i686 libaio.i686 libstdc++.i686
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.njupt.edu.cn
 * extras: mirrors.aliyun.com
 * updates: mirrors.njupt.edu.cn
base                                                             | 3.6 kB  00:00:00
extras                                                           | 3.4 kB  00:00:00
ol7_UEKR4                                                        | 1.2 kB  00:00:00
ol7_latest                                                       | 1.4 kB  00:00:00
updates                                                          | 3.4 kB  00:00:00
Resolving Dependencies
--> Running transaction check
---> Package binutils.x86_64 0:2.27-28.base.el7_5.1 will be updated
---> Package binutils.x86_64 0:2.27-34.base.0.1.el7 will be an update
---> Package glibc.i686 0:2.17-260.0.9.el7 will be installed
--> Processing Dependency: libfreebl3.so for package: glibc-2.17-260.0.9.el7.i686
--> Processing Dependency: libfreebl3.so(NSSRAWHASH_3.12.3) for package: glibc-2.17-260.0.9.el7.i686
---> Package glibc-devel.i686 0:2.17-260.0.9.el7 will be installed
---> Package libXtst.i686 0:1.2.3-1.el7 will be installed
--> Processing Dependency: libXi.so.6 for package: libXtst-1.2.3-1.el7.i686
--> Processing Dependency: libXext.so.6 for package: libXtst-1.2.3-1.el7.i686
--> Processing Dependency: libX11.so.6 for package: libXtst-1.2.3-1.el7.i686
---> Package libaio.i686 0:0.3.109-13.el7 will be installed
---> Package libstdc++.i686 0:4.8.5-36.0.1.el7 will be installed
--> Processing Dependency: libgcc_s.so.1(GCC_3.3) for package: libstdc++-4.8.5-36.0.1.el7.i686
--> Processing Dependency: libgcc_s.so.1(GCC_3.0) for package: libstdc++-4.8.5-36.0.1.el7.i686
--> Processing Dependency: libgcc_s.so.1(GCC_4.2.0) for package: libstdc++-4.8.5-36.0.1.el7.i686
--> Processing Dependency: libgcc_s.so.1 for package: libstdc++-4.8.5-36.0.1.el7.i686
--> Processing Dependency: libgcc_s.so.1(GLIBC_2.0) for package: libstdc++-4.8.5-36.0.1.el7.i686
--> Running transaction check
---> Package libX11.i686 0:1.6.5-2.el7 will be installed
--> Processing Dependency: libxcb.so.1 for package: libX11-1.6.5-2.el7.i686
---> Package libXext.i686 0:1.3.3-3.el7 will be installed
---> Package libXi.i686 0:1.7.9-1.el7 will be installed
---> Package libgcc.i686 0:4.8.5-36.0.1.el7 will be installed
---> Package nss-softokn-freebl.x86_64 0:3.36.0-5.el7_5 will be updated
---> Package nss-softokn-freebl.i686 0:3.36.0-5.0.1.el7_5 will be installed
---> Package nss-softokn-freebl.x86_64 0:3.36.0-5.0.1.el7_5 will be an update
--> Running transaction check
---> Package libxcb.i686 0:1.13-1.el7 will be installed
--> Processing Dependency: libXau.so.6 for package: libxcb-1.13-1.el7.i686
--> Running transaction check
---> Package libXau.i686 0:1.0.8-2.1.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

========================================================================================
 Package                 Arch        Version                      Repository       Size
========================================================================================
Installing:
 glibc                   i686        2.17-260.0.9.el7             ol7_latest      4.3 M
 glibc-devel             i686        2.17-260.0.9.el7             ol7_latest      1.1 M
 libXtst                 i686        1.2.3-1.el7                  base             20 k
 libaio                  i686        0.3.109-13.el7               base             24 k
 libstdc++               i686        4.8.5-36.0.1.el7             ol7_latest      317 k
Updating:
 binutils                x86_64      2.27-34.base.0.1.el7         ol7_latest      5.9 M
Installing for dependencies:
 libX11                  i686        1.6.5-2.el7                  ol7_latest      610 k
 libXau                  i686        1.0.8-2.1.el7                base             29 k
 libXext                 i686        1.3.3-3.el7                  base             39 k
 libXi                   i686        1.7.9-1.el7                  base             40 k
 libgcc                  i686        4.8.5-36.0.1.el7             ol7_latest      109 k
 libxcb                  i686        1.13-1.el7                   ol7_latest      229 k
 nss-softokn-freebl      i686        3.36.0-5.0.1.el7_5           ol7_latest      212 k
Updating for dependencies:
 nss-softokn-freebl      x86_64      3.36.0-5.0.1.el7_5           ol7_latest      222 k

Transaction Summary
========================================================================================
Install  5 Packages (+7 Dependent packages)
Upgrade  1 Package  (+1 Dependent package)

Total download size: 13 M
Is this ok [y/d/N]: y
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/14): glibc-2.17-260.0.9.el7.i686.rpm                          | 4.3 MB  00:00:02
(2/14): glibc-devel-2.17-260.0.9.el7.i686.rpm                    | 1.1 MB  00:00:00
(3/14): binutils-2.27-34.base.0.1.el7.x86_64.rpm                 | 5.9 MB  00:00:03
(4/14): libX11-1.6.5-2.el7.i686.rpm                              | 610 kB  00:00:00
(5/14): libXau-1.0.8-2.1.el7.i686.rpm                            |  29 kB  00:00:00
(6/14): libgcc-4.8.5-36.0.1.el7.i686.rpm                         | 109 kB  00:00:00
(7/14): libXi-1.7.9-1.el7.i686.rpm                               |  40 kB  00:00:00
(8/14): libstdc++-4.8.5-36.0.1.el7.i686.rpm                      | 317 kB  00:00:00
(9/14): libXext-1.3.3-3.el7.i686.rpm                             |  39 kB  00:00:00
(10/14): nss-softokn-freebl-3.36.0-5.0.1.el7_5.i686.rpm          | 212 kB  00:00:00
(11/14): nss-softokn-freebl-3.36.0-5.0.1.el7_5.x86_64.rpm        | 222 kB  00:00:00
(12/14): libxcb-1.13-1.el7.i686.rpm                              | 229 kB  00:00:00
(13/14): libXtst-1.2.3-1.el7.i686.rpm                            |  20 kB  00:00:02
(14/14): libaio-0.3.109-13.el7.i686.rpm                          |  24 kB  00:00:02
----------------------------------------------------------------------------------------
Total                                                      2.0 MB/s |  13 MB  00:06
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : libgcc-4.8.5-36.0.1.el7.i686                                        1/16
  Installing : nss-softokn-freebl-3.36.0-5.0.1.el7_5.i686                          2/16
  Installing : glibc-2.17-260.0.9.el7.i686                                         3/16
  Updating   : nss-softokn-freebl-3.36.0-5.0.1.el7_5.x86_64                        4/16
  Updating   : binutils-2.27-34.base.0.1.el7.x86_64                                5/16
  Installing : glibc-devel-2.17-260.0.9.el7.i686                                   6/16
  Installing : libXau-1.0.8-2.1.el7.i686                                           7/16
  Installing : libxcb-1.13-1.el7.i686                                              8/16
  Installing : libX11-1.6.5-2.el7.i686                                             9/16
  Installing : libXext-1.3.3-3.el7.i686                                           10/16
  Installing : libXi-1.7.9-1.el7.i686                                             11/16
  Installing : libXtst-1.2.3-1.el7.i686                                           12/16
  Installing : libstdc++-4.8.5-36.0.1.el7.i686                                    13/16
  Installing : libaio-0.3.109-13.el7.i686                                         14/16
  Cleanup    : binutils-2.27-28.base.el7_5.1.x86_64                               15/16
  Cleanup    : nss-softokn-freebl-3.36.0-5.el7_5.x86_64                           16/16
  Verifying  : libXtst-1.2.3-1.el7.i686                                            1/16
  Verifying  : libXext-1.3.3-3.el7.i686                                            2/16
  Verifying  : libstdc++-4.8.5-36.0.1.el7.i686                                     3/16
  Verifying  : libgcc-4.8.5-36.0.1.el7.i686                                        4/16
  Verifying  : libXi-1.7.9-1.el7.i686                                              5/16
  Verifying  : libxcb-1.13-1.el7.i686                                              6/16
  Verifying  : nss-softokn-freebl-3.36.0-5.0.1.el7_5.x86_64                        7/16
  Verifying  : libXau-1.0.8-2.1.el7.i686                                           8/16
  Verifying  : libaio-0.3.109-13.el7.i686                                          9/16
  Verifying  : binutils-2.27-34.base.0.1.el7.x86_64                               10/16
  Verifying  : glibc-devel-2.17-260.0.9.el7.i686                                  11/16
  Verifying  : glibc-2.17-260.0.9.el7.i686                                        12/16
  Verifying  : libX11-1.6.5-2.el7.i686                                            13/16
  Verifying  : nss-softokn-freebl-3.36.0-5.0.1.el7_5.i686                         14/16
  Verifying  : nss-softokn-freebl-3.36.0-5.el7_5.x86_64                           15/16
  Verifying  : binutils-2.27-28.base.el7_5.1.x86_64                               16/16

Installed:
  glibc.i686 0:2.17-260.0.9.el7             glibc-devel.i686 0:2.17-260.0.9.el7
  libXtst.i686 0:1.2.3-1.el7                libaio.i686 0:0.3.109-13.el7
  libstdc++.i686 0:4.8.5-36.0.1.el7

Dependency Installed:
  libX11.i686 0:1.6.5-2.el7                           libXau.i686 0:1.0.8-2.1.el7
  libXext.i686 0:1.3.3-3.el7                          libXi.i686 0:1.7.9-1.el7
  libgcc.i686 0:4.8.5-36.0.1.el7                      libxcb.i686 0:1.13-1.el7
  nss-softokn-freebl.i686 0:3.36.0-5.0.1.el7_5

Updated:
  binutils.x86_64 0:2.27-34.base.0.1.el7

Dependency Updated:
  nss-softokn-freebl.x86_64 0:3.36.0-5.0.1.el7_5

Complete!
```

### 创建目录和部署安装文件
以oracle用户，创建EM安装文件保存目录，并将安装文件拷贝到该目录中：
```shell
[oracle@oem ~]$ mkdir -p /u01/stage/em12104
[oracle@oem ~]$ cd /u01/stage/em12104/
[oracle@oem em12104]$ ll
[oracle@oem em12104]$ ll
total 6640888
-rw-r--r--. 1 oracle oinstall 2195693096 Nov 13 11:32 em12104_linux64_disk1.zip
-rw-r--r--. 1 oracle oinstall 1877449643 Nov 13 11:33 em12104_linux64_disk2.zip
-rw-r--r--. 1 oracle oinstall 2727123784 Nov 13 11:33 em12104_linux64_disk3.zip
```
依次解压缩安装文件：
```shell
[oracle@oem em12104]$ unzip em12104_linux64_disk1.zip 
[oracle@oem em12104]$ unzip em12104_linux64_disk2.zip 
[oracle@oem em12104]$ unzip em12104_linux64_disk3.zip 
[oracle@oem em12104]$ ll
total 1494528
drwxr-xr-x. 4 oracle oinstall         58 Jan 15  2014 bipruntime
drwxr-xr-x. 7 oracle oinstall       4096 May 24  2014 install
drwxr-xr-x. 4 oracle oinstall         39 May 24  2014 jdk
drwxrwxr-x. 4 oracle oinstall         30 May 24  2014 libskgxn
drwxr-xr-x. 4 oracle oinstall         39 May 24  2014 oms
drwxr-xr-x. 2 oracle oinstall       4096 Nov 13 11:36 plugins
-rw-r--r--. 1 oracle oinstall      42623 May 26  2014 release_notes.pdf
drwxrwxr-x. 2 oracle oinstall         96 May 24  2014 response
-rwxr-xr-x. 1 oracle oinstall       5375 Dec 26  2013 runInstaller
drwxr-xr-x. 9 oracle oinstall        232 May 24  2014 stage
drwxrwxr-x. 2 oracle oinstall         33 Nov 13 11:36 wls
-rwxr-xr-x. 1 oracle oinstall 1530333315 May 24  2014 WT.zip
```
以oracle用户创建oms和agent的安装目录:
```shell
[oracle@oem em12104]$ mkdir -p /u01/app/oracle/product/MiddlewareR4
[oracle@oem em12104]$ mkdir -p /u01/app/oracle/product/agentr4
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 400 MB.   Actual 4963 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 4064 MB    Passed
Checking monitor: must be configured to display at least 256 colors.    Actual 16777216    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2018-11-13_11-39-20AM. Please wait ...
```

### 开始安装EM
mac 打开一个新的终端，远程连接服务器GUI界面安装 EM：
```shell
[mac]ssh -Y oracle@192.168.12.xxx
oracle@192.168.12.xxx's password:
Last login: Mon Nov 12 16:30:05 2018
/usr/bin/xauth:  file /home/oracle/.Xauthority does not exist
[oracle@oem ~]$ /u01/stage/em12104/runInstaller
```

#### My Oracle Support Details
不选择`I wish to receive security updates via My Oracle Support`，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-034008.png)

#### Software Updates
选择`Skip`，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-034105.png)

#### Prerequisite Checks
等待所有检查通过，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-062755.png)

>注：若检查结果为`Not Executed`，直接点击`Next`。

#### Installation Types
在 `Create a new Enterprise Manager System` 下，选择 `Advanced`，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-063214.png)

#### Installation Details
设置如下：
- Middleware Home Location: `/u01/app/oracle/product/MiddlewareR4`
- Agent Base directory: `/u01/app/oracle/product/agentr4`
- Host Name: `oem.com` (注意，必须是FQDN)

点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-063459.png)


#### Select Plug-ins
除了默认的选择之外，选择如下三个插件：
- Oracle Cloud Application
- Oracle Consolidation Planning and Chargeback
- Oracle Virtualization

然后点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-064737.png)

#### WebLogic Server Configuration Details
输入WebLogic GCDomain的密码，以及Node Manager密码。其他保持默认选择，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-065218.png)

> 注意：密码不能以数字开头！

#### Database Connection Details
输入数据库的连接信息：
- Database Host Name: `oem.com`
- Port: `1521`
- Service/SID: `omr`
- SYS Password: `yourpassword`

Deployment Size选择`SMALL`。关于Deployment Size各个选项的含义，可以参考以下说明：
- Small < 100 agents < 1000 targets
- Medium < 1000 agents < 10,000 targets
- Large > 1000 agents > 10,000 targets

点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-065426.png)

安装向导可能还会检查出一些错误的数据库设置，这时按照向导中的提示选择自动修复即可：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-065530.png)

可能还会有一些警告出现。这是一些可能会影响EM的运行性能的警告，不会对EM的正常启动造成影响，所以可以暂时忽略，可以在安装完之后再修改数据库配置文件。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-065753.png)

#### Enterprise Manager Configuration Details
输入SYSMAN和OMA Agent Registration的密码。其他的选项保持默认值，点击`Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-070202.png)

> 注意：密码不能以数字开始，而且在安装完成后登陆EM的用户名就是sysman（不区分大小写）。

#### Port Configuration Details
保持默认值，点击 `Next`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-070307.png)

#### Review
检查所有设置，确认无误的话，点击`Install`。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-070446.png)

#### Installation Progress Details
开始漫长的安装过程，请耐心等待。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-080116.png)

> 一定要确保根目录磁盘空间够用，建议大于40G，否则在安装过程可能会报错，比如`start oralce management service failed`。当出现这个错误时，我在网上找了各种解决方案都无法解决，最后发现是根目录磁盘空间不足造成的。

最后需要以root用户运行一个脚本：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-14-oem12104-15.png)
```shell
[root@oem lib]# /u01/app/oracle/product/MiddlewareR4/oms/allroot.sh

Starting to execute allroot.sh .........

Starting to execute /u01/app/oracle/product/MiddlewareR4/oms/root.sh ......
Running Oracle 11g root.sh script...

The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /u01/app/oracle/product/MiddlewareR4/oms

Enter the full pathname of the local bin directory: [/usr/local/bin]:
The file "dbhome" already exists in /usr/local/bin.  Overwrite it? (y/n)
[n]: y
   Copying dbhome to /usr/local/bin ...
The file "oraenv" already exists in /usr/local/bin.  Overwrite it? (y/n)
[n]: y
   Copying oraenv to /usr/local/bin ...
The file "coraenv" already exists in /usr/local/bin.  Overwrite it? (y/n)
[n]: y
   Copying coraenv to /usr/local/bin ...

Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root.sh script.
Now product-specific root actions will be performed.
/etc exist

Creating /etc/oragchomelist file...
/u01/app/oracle/product/MiddlewareR4/oms
Finished execution of  /u01/app/oracle/product/MiddlewareR4/oms/root.sh ......


Starting to execute /u01/app/oracle/product/agentr4/core/12.1.0.4.0/root.sh ......
Finished product-specific root actions.
/etc exist
Finished execution of  /u01/app/oracle/product/agentr4/core/12.1.0.4.0/root.sh ......
```

##### 可能出现的问题
在 `Installation Progress Details` 这一步，可能会出现：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-13-112913.png)

```
Error in invoking target 'install' of makefile '/u01/app/oracle/product/MiddlewareR4/Oracle_WT/webcache/lib/ins_calypso.mk'. See '/u01/app/oraInventory/logs/cloneActions2018-11-13_03-14-34-PM.log' for details.
```
解决方案（参考了[此文](http://www.dbaglobe.com/2015/10/fix-3-installation-issues-on-oem12c.html)）
增加 `-ldms2` 到 `/u01/app/oracle/product/MiddlewareR4/Oracle_WT/lib/sysliblist` 中。即将文件`sysliblist`的内容修改为：
```shell
[root@oem ~]# cd /u01/app/oracle/product/MiddlewareR4/Oracle_WT/lib
[root@oem lib]# cp -p ./sysliblist ./sysliblist.orig
[root@oem lib]# vi ./sysliblist
-ldl -lm -lpthread -lnsl -lirc -lipgo -ldms2
```


#### Finish
安装结束。
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-14-013130.png)

在安装结束画面中会显示下面的信息：

```
This information is also available at: 

	/u01/app/oracle/product/MiddlewareR4/oms/install/setupinfo.txt

See below for information pertaining to your Enterprise Manager installation:


Use the following URL to access:

	1. Enterprise Manager Cloud Control URL: https://oem.com:7803/em
	2. Admin Server URL: https://oem.com:7102/console

The following details need to be provided during the additional OMS install:

	1. Admin Server Hostname: oem.com
	2. Admin Server Port: 7102

You can find the details on ports used by this deployment at : /u01/app/oracle/product/MiddlewareR4/oms/install/portlist.ini


 NOTE:
 An encryption key has been generated to encrypt sensitive data in the Management Repository. If this key is lost, all encrypted data in the Repository becomes unusable. 

 A backup of the OMS configuration is available in /u01/app/oracle/product/gc_inst/em/EMGC_OMS1/sysman/backup on host oem.com. See Cloud Control Administrators Guide for details on how to back up and recover an OMS.

 NOTE: This backup is valid only for the initial OMS configuration. For example, it will not reflect plug-ins installed later, topology changes like the addition of a load balancer, or changes to other properties made using emctl or emcli. Backups should be created on a regular basis to ensure they capture the current OMS configuration. Use the following command to backup the OMS configuration:
/u01/app/oracle/product/MiddlewareR4/oms/bin/emctl exportconfig oms -dir <backup dir> 
```

也就是说，可以从下面的URL访问到EM了：
```
https://oem.com:7803/em
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-11-14-Screen_Shot_2018-11-14_at_11_06_32.png)

至此，EM的全部安装过程结束。


##### 可能出现的问题
若经过上述步骤后，仍旧无法访问`https://oem.com:7803/em`，可能是因为没有关闭防火墙，导致该端口无法被访问。可通过如下方式验证：
```shell
[mac]telnet 192.168.12.xxx 7803
Trying 192.168.12.xxx...
telnet: connect to address 192.168.12.xxx: Connection refused
telnet: Unable to connect to remote host
```
关闭防火墙：
```shell
[root@oem lib]# systemctl stop firewalld.service
[root@oem lib]# firewall-cmd --state
not running
```
此时再次验证`7803`，则可以被访问了，且页面`https://oem.com:7803/em`也能访问了。
```shell
[mac]telnet 192.168.12.XXX 7803
Trying 192.168.12.xxx...
Connected to localhost.
Escape character is '^]'.
```
(END)

## 参考文献

- [Oracle Enterprise Manager 12c 安装过程，by 夕阳下的奔跑](http://xintq.net/2015/08/04/oracle-enterprise-manager-12c-installation/)
- [Oracle Database 12c Installation Procedure](http://people.fas.harvard.edu/~zzhao/cscie67/tasection1_2.pdf)
- [Oracle Database 12c Installation on CentOS 7, by centos.org](https://wiki.centos.org/HowTos/Oracle12onCentos7#head-539cfe4ae2bd6f126de557cbfc5d7d7a99826c04)
- [How to Install Oracle Database 12c on CentOS 7, by howtoforge](https://www.howtoforge.com/tutorial/how-to-install-oracle-database-12c-on-centos-7/#step-configure-desktop)
- [How to Install Oracle Database 12c on RHEL/CentOS 7](https://www.tecmint.com/install-oracle-database-12c-on-centos-7/)
- [深入理解Linux修改hostname, by 潇湘隐者](http://www.cnblogs.com/kerrycode/p/3595724.html)