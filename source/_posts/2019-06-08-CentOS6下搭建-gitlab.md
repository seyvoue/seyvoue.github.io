---
title: '@CentOS6下搭建 gitlab'
comments: true
categories: manual
tags: git
abbrlink: 20173b43
date: 2019-06-08 20:56:02
updated:
mathjax:
keywords:
description:
---

step1. 安装相关的依赖
```shell
[root@localhost ~]# yum install -y curl policycoreutils-python openssh-server cronie
```

step2. 安装邮件服务，亦可使用别的邮件服务，如：`smtp` 等，可参考[此文](https://docs.gitlab.com/omnibus/settings/smtp.html)。       
```shell
[root@localhost ~]# yum install -y postfix
[root@localhost ~]# service postfix start
[root@localhost ~]# chkconfig postfix on
```

step3. 安装 gitlab
```shell
# 下载安装脚本，配置gitlab 源
[root@localhost ~]# curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | bash
# 安装 gitlab-ee
[root@localhost ~]# yum install -y gitlab-ee
```

step4. 修改 `/etc/gitlab/gitlab.rb`，配置 gitlab 并启动
```vim
vi /etc/gitlab/gitlab.rb
#修改访问地址以及默认端口，为如下：
external_url 'http://gitlab.seyvoue.com'
unicorn['port'] = 18080
#初始化 gitlab
gitlab-ctl reconfigure
#启动 gitlab
gitlab-ctl start
#查看 gitlab 状态
gitlab-ctl status
```

step5. 若 gitlab 布置在虚拟机中，需要在宿主机 `hosts` 文件增加：
```vim
虚拟机IP external_url
```

step6. 验证。在浏览器中访问 `http://gitlab.seyvoue.com/` 即可

参看链接
* [Omnibus package installation](https://about.gitlab.com/install/#centos-6)
* [从零开始搭建Gitlab服务器](https://www.jianshu.com/p/43860be68b52)
* [搭建Gitlab+maven+jenkins持续集成环境](https://www.jianshu.com/p/3507d8b2ac87)
