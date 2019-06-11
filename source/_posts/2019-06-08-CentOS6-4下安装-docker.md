---
title: '@CentOS6.4下安装 docker'
comments: true
categories: manual
tags: docker
abbrlink: 9cb3f9b7
date: 2019-06-08 20:55:01
updated:
mathjax:
keywords:
description:
---

 <!--more-->

step1. 创建用户 `user`，用于之后 docker 启动、停止、运行的默认用户
```shell
#创建用户 user
[root@localhost ~]# useradd user
[root@localhost ~]# passwd user
#给 user sudo 权限
[root@localhost ~]# visudo
add the following line
user   ALL=(ALL)       NOPASSWD: ALL
```

step2. 配置 docker yum 源
```shell
[root@localhost ~]# vi /etc/yum.repo.d/docker.repo
[docker-repo]
name=Docker Repo
baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
```

step3. 安装 `docker-engine`，并将其配置为系统服务
```shell
[root@localhost ~]# su - user
[user@localhost ~]$ sudo yum install docker-engine
[user@localhost ~]$ sudo chkconfig docker on
[user@localhost ~]$ sudo service docker start
[user@localhost ~]$ sudo service docker status
docker (pid 3389) is running…
[user@localhost ~]$ ps aux | grep docker
root       3189  3.0  0.5 466152 22552 ?        Sl   05:26   0:16 /usr/bin/docker -d
user       3568  0.0  0.0 103232   864 pts/0    S+   05:35   0:00 grep docker
```
注：若出现以下情况，需要升级 `device-mapper-libs` 包，如下
```shell
[user@localhost ~]$ sudo service docker stauts
docker dead but pid exists
#若出现以上的情形，可以通过升级 device-mapper-libs 包，或者 yum update -y 的方式更细系统版本到最新版本
[user@localhost ~]$ sudo yum update -y device-mapper-libs
```

step4. 将 `user` 添加到 `docker` 用户组中，并验证是否可通过 `user` 用户使用 docker 基本功能
```shell
[user@localhost ~]$ sudo usermod -aG docker user
[user@localhost ~]$ id user
uid=501(user) gid=501(user) groups=501(user),492(docker)
#切到 root 用户停止 docker 服务
[user@localhost ~]$ exit
[root@localhost ~]# service docker stop
#切回到 user 用户，验证是否可使用 docker 基本功能
[root@localhost ~]# su - user
[user@localhost ~]$ sudo service docker restart
[user@localhost ~]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
```

step5. 下载 centos 镜像，创建并启动容器
```
[user@localhost ~]$ docker pull centos:centos6
[user@localhost ~]$ docker pull centos:latest
[user@localhost ~]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
centos              centos6             4f2ed42dccff        12 weeks ago        193.9 MB
centos              latest              ee2526f4865b        12 weeks ago        201.8 MB
[user@localhost ~]$ docker run -it —name test_docker centos:centos6 /bin/bash
[root@34d076f4dac3 /]# exit
exit
[user@localhost ~]$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
34d076f4dac3        4f2ed42dccff        "/bin/bash"         14 seconds ago      Exited (0) 3 seconds ago                       test_docker
```

参考文献
* [Install docker on CentOS 6 or 7](http://mazzakolinux.com/7066-2/)
