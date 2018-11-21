---
title: '@解决Hexo对mathjax中部分公式无法正常解析的问题'
categories: guildlines
abbrlink: 8d6d20a0
date: 2017-08-28 21:13:35
updated: 2018-02-01 18:10:00
tags: hexo
keywords:
description:
mathjax: true
comments: true
---


> 解决Hexo和MathJax的兼容问题。

<!-- more -->

## 问题

先重现以下问题，在写博文的时候，我码了一个公式：

```mathjax
$$J(\theta_0,\theta_1)=\frac{1}{2m} \sum_{i=1}^{m} ( h_{\theta}(x^{(i)})-y^{(i)} )^2 \quad (for \, j=0 \; and \; j=1)$$
```

如果一切正常的话，应该渲染成这样：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2017-08-28-130723.jpg)

但事与愿违，无法正常解析。

## 解决方案

简单来说，要让你的Hexo支持MathJax渲染公式，你只需要使用两条命令：
To fully support MathJax in your Hexo blog, you can simply use the following commands:

- `npm uninstall hexo-renderer-marked --save`
- `npm install hexo-renderer-kramed --save`

第一条命令用于卸载 `hexo-renderer-marked`（注意，如果你使用了其他的渲染插件，请卸载对应的插件），它是hexo自带的Markdown渲染引擎。
The first command uninstall Hexo’s default Markdown renderer.

第二条命令用于安装 `hexo-renderer-kramed` 插件，这个渲染插件针对MathJax支持进行了改进。安装完成后，重新生成博客就会惊喜地发现你的公式已经能够正常显示了。
The second command install new Markdown renderer which can support MathJax fully. After installation, you should regenerate your blog to see the changes.


## 参考链接

- [如何处理Hexo和MathJax的兼容问题 | 林肯先生的Blog
](http://2wildkids.com/2016/10/06/如何处理Hexo和MathJax的兼容问题/#小结)

