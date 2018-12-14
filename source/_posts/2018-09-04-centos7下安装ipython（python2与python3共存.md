---
title: '@centos7下安装ipython（python2与python3共存)'
comments: true
categories: manual
tags: python
abbrlink: b8430542
date: 2018-09-04 13:19:37
updated:
keywords:
description:
---

安装 ipython 前，你需要预安装：
- epel-release
- python
- pip

linux 发行版默认是已安装 `python2.7`的，所以我们只需安装 python3.

**step1: install epel-release**
```shell
[root@localhost ~]# yum install epel-release
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-041745.png)

**step2: install python3**
```shell
[root@localhost ~]# yum install python34
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-042221.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-042301.png)

**step3: 验证 python 是否安装成功**
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-042454.png)

**step4: install pip and pip3**
```shell
[root@localhost ~]# yum install python2-pip
[root@localhost ~]# yum install python34-pip
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-042749.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-042855.png)

**step5: 验证 pip 是否安装成功**
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-043045.png)

**step6: install ipython**
```shell
[root@localhost ~]# pip install ipython
[root@localhost ~]# pip3 install ipython
```

**step7: 验证 ipython 是否安装成功**
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-051546.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-09-04-051701.png)

