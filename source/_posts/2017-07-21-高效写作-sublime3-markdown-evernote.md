---
title: '@高效写作 sublime3+markdown+evernote'
comments: true
categories: guildlines
tags: markdown
abbrlink: b67313b
date: 2017-07-21 18:28:05
updated: 2018-01-18 03:07:03
keywords: tools
description:
---


本文介绍在 SublimeText3(下文统一简称为 ST3)下用 markdown 语法写文章/笔记，并同步到 Evernote 中。

对于有强迫症的我来说，一直在寻找一个工具，具备以下功能：

- 手可以不离开键盘(即尽可能的不需要用鼠标进行操作)，便可以实现，如：加粗，多级标题，表格，列表，加粗，斜体，数学公式等基本格式
- 写好的笔记不仅可以保留在本地，同时还可以自动同步到 evernote 以做备份
- 类似于系统自带的文本编辑器，界面简单，且支持打开各种文本格式的文件
- 敲代码与写作两不误，不再需要在各种软件间频繁的切换
- ...

Markdown 是一种非常方便非常容易上手的写作格式，你只要记住十来种最多几十种大不了上百种语法，就可以轻松地写出各种格式的文章了；
Evernote 是个笔记软件，其功能类似于 Microsoft 的 OneNote；

---

## 下载安装 SublimeText3

step1:  下载[SublimeText3](https://www.sublimetext.com/3)  
step2:  在 ST3 中安装 `package control` 插件，安装方法参考[这篇文章](https://packagecontrol.io/installation)

## 配置 SublimeText 

安装以下插件:  

__[MarkdownEditing](https://packagecontrol.io/packages/MarkdownEditing)__    
> Markdown 编辑及语法高亮支持  

__[MarkdownExtended](https://packagecontrol.io/packages/Markdown%20Extended)__ 
> Markdown syntax highlighter for Sublime Text, with extended support for GFM fenced code blocks.

__[MonokaiExtended](https://packagecontrol.io/packages/Monokai%20Extended)__
> 不错的 markdown 主题，支持多种语言高亮

__[MarkdownPreview](https://packagecontrol.io/packages/Markdown%20Preview)__  
Mac 用户依次点击菜单栏中的`Preferences -> Package Settings -> Markdown Preview -> Settings-User`打开用户设置文件，加入如下内容： 

```JSON
{
    "parser": "github",
    "build_action": "browser",
    "enable_mathjax": true,
    "enable_uml": true,
    "enable_highlight": true,
    "enable_pygments": true,
    "enabled_parsers": ["github"],
    "github_mode": "markdown",
    "github_inject_header_ids": true,
    "enable_autoreload": false,
    "enable_mathjax": true,
    "enable_highlight": true,
    "enabled_extensions": [
            "extra",
            "github",
            "toc",
            "headerid",
            "meta",
            "sane_lists",
            "smarty",
            "wikilinks",
            "admonition",
            "codehilite(guess_lang=False,pygments_style=emacs)"
        ]
}

```

__[Table Editor](https://packagecontrol.io/packages/Table%20Editor)__   
> Markdown中的表格书写体验很差，所以有人为这个开发了一个插件，具有较好的自适应性，会自动对齐，就像在 Excel 中编辑表格一样  

__[MarkdownTOC](https://packagecontrol.io/packages/MarkdownTOC)__ 
> 根据文章中的标题层次，在文章的开头自动生成目录

Mac 用户依次点击菜单栏中的`Preferences -> Package Settings -> Markdown Preview -> Settings-User`打开用户设置文件，加入如下内容：

```JSON
{
  "default_autolink": true,
  "default_bracket": "round",
  "default_depth": 0
}
```

__[Evernote](https://packagecontrol.io/packages/Evernote)__
> 支持将文章同步到 Evernote 的指定笔记本中，且支持增量更新编辑过的内容。


---

## 参考
1. [SublimeText下写作利器之MarkdownEditing](http://jeffjade.com/2015/08/28/2015-08-28-Write-Morkdown/index.html)
2. [打造軟體工程師的個人筆記: Sublime Text 3 + Evernote + Markdown](https://medium.com/@KenjiChao/打造軟體工程師的個人筆記-sublime-text-3-evernote-markdown-39c10d170231)
3. [用 SublimeText3编写 markdown 文档](http://blog.kamidox.com/write-markdown-using-sublime.html#_16)
4. [markdown 语法手册](http://blog.leanote.com/post/freewalk/Markdown-%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C)
5. [让文档回归本质，为什么应该用Markdown](http://jolestar.com/markdown-tools/)

