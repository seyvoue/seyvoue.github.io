---
title: '@Postman 如何使用(mac)'
comments: true
categories: tutorial
abbrlink: f7bab520
date: 2018-02-02 11:58:49
updated:
tags:
keywords:
description:
---

Postman 可以做什么？

- 创建 + 测试: 创建和发送任何的HTTP请求，请求可以保存到历史中再次执行
- Organize: 使用Postman Collections 为更有效的测试及集成工作流管理和组织APIs
- document: 依据你创建的 Clollections 自动生成API文档,并将其发布成规范的格式
- collarorate: 通过同步连接你的team和你的api，以及权限控制，API库

<!--more-->

## 安装 Postman

方式一：安装独立APP（推荐）：https://www.getpostman.com/

方式二：安装 chrome 浏览器版：

- 首先安装 postman 浏览器版 [ _*download*_ ](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?utm_source=chrome-ntp-icon)
- 然后安装 postman-interceptor extension [ _*download*_ ](https://chrome.google.com/webstore/detail/postman-interceptor/aicmkgpgakddgnaphhhpliifpcfhicfo?utm_source=chrome-ntp-icon)


## 使用方法（以独立APP举例，chrome 插件类似）

### 设置正确的 COOKIE

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-26-47.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-21-27.png)

### GET

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-21-25.png)

### POST(json)

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-30-55.png)

### POST(formdata)

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-33-56.png)

其他的请求类型，例如 DELETE 也是同样的用法。

### 历史记录

Postman 默认会记录所有的执行历史，如果你注册帐号并登录，历史记录还会同步。

同时，Postman还提供Collections功能，对请求进行分类整理。

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-36-36.png)

### 环境变量

Postman 提供一个 Environments 功能，类似与系统变量，可以一键切换测试环境。

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-43-9.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-02-02-image2017-10-23_18-44-37.png)

## 其它

Postman 还可以提供接口批量测试 等功能，有兴趣的话可以自己多玩玩。

Hosts修改工具：[iHost](https://itunes.apple.com/cn/app/ihosts-%E7%BC%96%E8%BE%91%E7%A5%9E%E5%99%A8/id1102004240?mt=12)

## 参考链接

- [基于Postman的API自动化测试](https://segmentfault.com/a/1190000005055899)
- [Postman 使用详解--掘金](http://lucia.xicp.cn/2016/05/21/test/postman%E7%AC%94%E8%AE%B0/)