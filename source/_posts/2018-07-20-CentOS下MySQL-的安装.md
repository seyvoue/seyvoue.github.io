---
title: '@CentOS下MySQL 的安装'
comments: true
categories: guildlines
tags: mysql
abbrlink: 5e99fb6d
date: 2018-07-20 00:00:57
updated:
keywords:
description:
---

```shell
# 下载链接可从官网获得
[root@localhost ~]# wget https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
[root@localhost ~]# rpm -ivh mysql80-community-release-el7-1.noarch.rpm
[root@localhost ~]# yum install mysql-community-server
# 启动 MySQL 服务
[root@localhost ~]# service mysqld start
# 检查 MySQL 服务是否已正常启动
[root@localhost ~]# service mysqld status
# 找到 MySQL 安装时给的默认密码
[root@localhost ~]# grep 'temporary password' /var/log/mysqld.log
[root@localhost ~]# mysql -uroot -p
# 修改密码，密码得包含大小写数字和符号，不然可能过不了
[root@localhost ~]# ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!'
```

## 参考链接

- [Installing MySQL on Linux Using the MySQL Yum Repository](https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html)
