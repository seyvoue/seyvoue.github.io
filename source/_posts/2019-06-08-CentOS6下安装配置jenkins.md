---
title: '@CentOS6下安装配置jenkins'
comments: true
categories: manual
tags: jenkins
abbrlink: 77bc273
date: 2019-06-08 20:55:39
updated:
mathjax:
keywords:
description:
---

<!--more-->

step1. 安装 Jenkins
```shell
[root@localhost ~]# yum install jenkins
#查看 Jenkins 安装路径
[root@localhost ~]# rpm -ql jenkins
```

step2. 修改默认端口 `8080`
```vim
[root@localhost ~]# vi /etc/sysconfig/jenkins
:set ignorecase
/jenkins_port
JENKINS_PORT=“8000”
```

注：若本机的 jdk 环境并非使用的是系统自带的 openjdk，而是通过在 /etc/profile 等中配置环境变量的方式安装的 jdk 环境的话，需要在 `/etc/init.d/jenkins` 中添加你自己安装的 jdk 路劲，
```vim
[root@localhost ~]# vi /etc/init.d/jenkins
candidates="
/usr/local/jdk1.8.0_121/bin/java <==在这里添加你的 jdk 路径
/etc/alternatives/java
/usr/lib/jvm/java-1.8.0/bin/java
/usr/lib/jvm/jre-1.8.0/bin/java
/usr/lib/jvm/java-1.7.0/bin/java
/usr/lib/jvm/jre-1.7.0/bin/java
/usr/bin/java
"
```
