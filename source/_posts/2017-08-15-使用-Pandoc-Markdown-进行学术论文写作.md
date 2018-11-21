---
title: '@使用 Pandoc Markdown 进行学术论文写作'
comments: true
categories: guildlines
abbrlink: fc12d89
date: 2017-08-15 17:22:19
updated: 2018-01-18 03:11:58
tags:
  - markdown
  - pandoc
  - latex
keywords:
description:
---


> 一个 LaTeX 的替代解决方案，并非完全要放弃 LaTeX。因为科技论文写作不可避免要有许多数学符号和公式。而且许多期刊都提供了 LaTeX 模板，因此如果这种替代方案需要让我完全放弃 LaTeX，我也会有些犹豫的。
> 一直有在用 Markdown 写笔记的习惯，意外的发现了 Markdown＋Pandoc 的组合，正是我所苦苦寻找的替代解决方案。

<!-- more -->

本科的论文是用 word 写的，无奈 word 在频繁修改文档时，表格和插图很容易乱掉，并且无法解决多人协助，版本管理问题。加上有纯文本癖，我很快将目光转到 LaTeX 上。用了一段时间 LaTeX ，表格和插图不再混乱，参考文献引用也完全没有问题，对数学公式的支持几乎可以用完美来形容，版本管理和多人协作也可以通过 Git 来实现。但也发现了一些 LaTeX 在创作时的问题：

- 写作时无法集中精力到内容上：使用 LaTeX 写作，特别当文档比较大并存在大量公式的时候，会存在一大堆的标签，文档结构不够清晰。
- 标签需要配对：虽说标签配对不是很大的问题。但编译报错，返回编辑器补全标签还是挺让人懊恼，容易打断写作思路。
- 无法做到所见即所得：所见即所得是 Word 做的比较好的地方，使用 LaTeX 进行写作的时候，你并不知道结果出来是什么样子的，一切只有编译以后才知道。你想修改 pdf 里的一句话，还需要找对应 tex 文件的所在行。虽然可以使用搜索，但是毕竟还是没有所见即所得来的方便。


*另外：之前写过一篇“[@高效写作 sublime3+markdown+evernote](http://www.seyvoue.com/posts/b67313b/)”，是关于如何将 `Sublime Text` 打造成一个 markdown 写作利器的，有兴趣的可以了解下。（其实，网上有很多现成的markdown编辑器，对于没有强迫症的人来说，会是更好的选择，比如：马克飞象，Mou，MWeb 等等）*

{% cq %}
未完待续
{% endcq %}

<style type="text/css">
.post-body .fancybox img.pic_styl {
    display: inline !important;
    height: 140px;
    width: auto;
}
</style>
