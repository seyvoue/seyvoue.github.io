---
title: '@用 GitHub 托管本地代码（MAC）'
comments: true
categories: guildlines
tags: git
abbrlink: 7a97ce34
date: 2018-01-21 00:24:42
updated: 2018-01-23 20:31:21
keywords:
description:
---

本文包括以下内容：

- 如何在本地安装 git
- 如何实现本机与 github 的 SSH 认证登录（即代码提交时，无需输入 github 密码）
- Git 的常用命令


## install git

- step1: install Xcode command-line tools 

```shell
xcode-select --install
```

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2017-08-03-104639.jpg)

另外，也可以直接安装 Xcode 软件。


- step2: install homebrew

如何安装请参考该文：[@HomeBrew 的安装与使用](http://seyvoue.com/posts/36d45381/#more)

- step3: install git with homebrew

```shell
 brew install git
```

## 设置 SSH Key

step1: 注册[Github](https://github.com) 账号

step2: 创建公私钥，参考这篇文章([@SSH远程登录的实现](http://seyvoue.com/posts/68483533/))

step3: 设置 SSH Key(在GitHub中添加公钥）

> 在本机连接 Github 上已有的仓库，是通过使用 SSH 方式认证的(不了解SSH的，可参考“[SSH原理与运用--阮一峰](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)”)。

执行下面的命令将公钥中的内容复制到剪贴板

```shell
$ pbcopy < ~/.ssh/id_rsa.pub
```

进入 Github 账号设置页，添加 SSH Key

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2017-08-03-Screen_Shot_2017-08-03_at_19_52_33.png)

至此，你便可以将项目提交到 github 上了。

## git 常用命令

下面列出了日常经常会用到的一些 git 指令。另外，可参考(“[@git 命令详解](http://seyvoue.com/posts/b6d2867a/#more)”)，了解git的更多操作。

```shell
# 将项目克隆岛本地
git clone 远程仓库的地址

# 在本地创建新的分支
git checkout -b YourBranchName

# 将本地代码同步到远程仓库同名分支（）
git add .
git commit -m '对本次更改的内容做个备注'
git push --set-upstream origin 远程分支名 # 第一次 push 时，可以指定本地分支绑定远程的某个分支
git push # 之后只需直接 push，就会默认同步到远程同名分支

# 更新本地仓库，使本地仓库保持最新（与远程仓库除本分支外保持一致）
git fetch origin # 默认取回远程所有分支的更新
git fetch origin branchname #取回远程指定分支的更新

# 将远程分支与本地分支合并
git pull origin remoteBranchName:localBranchName # 将远程分支与本地分支合并
git pull origin remoteBranchName # 远程分支与当前分支合并
```


## 参考链接

- [Git远程操作详解--阮一峰](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)


