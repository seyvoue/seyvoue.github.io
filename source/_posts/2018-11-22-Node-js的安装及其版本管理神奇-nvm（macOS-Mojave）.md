---
title: '@Node.js的安装及其版本管理神奇 nvm（macOS Mojave）'
comments: true
categories: guildlines
tags: nodejs
abbrlink: 9e29d568
date: 2018-11-22 21:52:32
updated:
mathjax:
keywords:
description:
---

建议使用 nvm 管理计算机中的 Node.js，如果你已经安装过 Node.js ，可以先删除,如何删除參考[这篇文章](http://phoeshow.github.io/2017/05/15/Mac%E5%88%A0%E9%99%A4nodejs%E7%9A%84%E6%96%B9%E6%B3%95/)。

<!--more-->

## 安装 nvm
step1: 使用 Homebrew 來安裝 nvm 
```shell
$ brew install nvm
```
step2: 修改`~/.zshrc`（因为笔者默认的 shell 为 zsh）
```
#nvm
export NVM_DIR="$HOME/.nvm"
source $(brew --prefix nvm)/nvm.sh
```
step3：验证是否安装成功
```
$ nvm --version
0.33.11
```


## 安装 Node.js

step1: 查看可安装的 node 版本
```shell
$ nvm ls-remote
        v0.1.14
        v0.1.15
        v0.1.16
        v0.1.17
        ...
        v8.11.4   (LTS: Carbon)
        v8.12.0   (LTS: Carbon)
        v8.13.0   (Latest LTS: Carbon)
        v10.10.0
       v10.11.0
       v10.12.0
       v10.13.0   (Latest LTS: Dubnium)
        ...
```

step2：安装 Node.js，这里安装 `10.13.0`
```shell
$ nvm install 10.13.0
```
也可以直接安装目前的稳定版：
```shell
$ nvm install stable
```

step3: 切换到想用的 Node.js 版本
```shell
$ nvm use 10.13.0
```

step4：验证 Node.js 是否安装成功
```shell
$ node -v
v10.13.0
```

`nvm` 会将 不同版本的 `node` 安装在`$NVM_DIR/versions/`目录下，如下：
```shell
$ type node
node is /Users/seyvoue/.nvm/versions/node/v10.13.0/bin/node
```