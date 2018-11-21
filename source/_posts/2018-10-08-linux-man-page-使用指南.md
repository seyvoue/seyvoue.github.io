---
title: '@linux man page 使用指南'
comments: true
categories: languages
tags: shell
abbrlink: 3c9b26ea
date: 2018-10-08 14:17:46
updated:
keywords:
description:
---

linux 命令那么多，且每个命令又有很多选项，难道要都背下来吗？笔者忘性大，可能当下还记得命令的各种详细用法，对于一些不常使用的命令，过段时间就忘记了具体用法了。所以，笔者选择只记住“**在什么场景，使用哪类命令**”，然后结合 man page，这种方式不仅快捷而且不易出错。接下来，本文将会对 man page 的页面结构做简单说明。

>TIPS: 进入man指令的功能后，你可以按下 **空格鍵** 往下翻頁，可以按下 `q` 按键來离开man的环境。 更多在man指令下的功能，将在本文的后续部分介绍！

<!--more-->

## 以 man date 为例

```
[zodas@localhost ~]$ man date
DATE(1)                          User Commands                         DATE(1) 
# 请注意上面这个括号内的数字 
NAME  <==这个命令的完整全名，如下所示为date且说明简单用途为配置与显示日期/时间 
       date - print or set the system date and time 
 
SYNOPSIS  <==这个命令的基本语法如下所示 
       date [OPTION]... [+FORMAT] 
       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]] 
 
DESCRIPTION  <==详细说明刚刚语法谈到的选项与参数的用法 
       Display  the  current  time  in  the given FORMAT, or set the system 
       date. 
 
       -d, --date=STRING  <==左边-d为短选项名称，右边--date为完整选项名称 
              display time described by STRING, not 'now' 
 
       -f, --file=DATEFILE 
              like --date once for each line of DATEFILE 
 
       -r, --reference=FILE 
              display the last modification time of FILE 
....(中间省略).... 
       # 找到了！底下就是格式化输出的详细数据！ 
       FORMAT controls the output.  The only valid option  for  the  second 
       form  specifies  Coordinated  Universal Time.  Interpreted sequences 
       are: 
 
       %%     a literal % 
 
       %a     locale's abbreviated weekday name (e.g., Sun) 
 
       %A     locale's full weekday name (e.g., Sunday) 
....(中间省略).... 
ENVIRONMENT  <==与这个命令相关的环境参数有如下的说明 
       TZ     Specifies the timezone, unless  overridden  by  command  line 
              parameters.   If  neither  is  specified,  the  setting  from 
              /etc/localtime is used. 
 
AUTHOR  <==这个命令的作者啦！ 
       Written by David MacKenzie. 
 
REPORTING BUGS  <==有问题请留言给底下的email的意思！ 
       Report bugs to <bug-coreutils@gnu.org>. 
 
COPYRIGHT  <==受到著作权法的保护！用的就是 GPL 了！ 
       Copyright ? 2006 Free Software Foundation, Inc. 
       This is free software.  You may redistribute copies of it under  the 
       terms      of      the      GNU      General      Public     License 
       <http://www.gnu.org/licenses/gpl.html>.  There is  NO  WARRANTY,  to 
       the extent permitted by law. 
 
SEE ALSO  <==这个重要，你还可以从哪里查到与date相关的说明文件之意 
       The  full  documentation for date is maintained as a Texinfo manual. 
       If the info and date programs are properly installed at  your  site, 
       the command 
 
              info date 
 
       should give you access to the complete manual. 
 
date 5.97                          May 2006                            DATE(1)
```

## man page 页面结构
基本上 man page 有好几个部分组成，如下表所示：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-10-08-060213.png)

对于 `DATE(1)` 括号中的数字1，表示一般使用者可以使用的命令，括号中的数字可以帮着我们快速的了解到所查询内容的属性。

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-10-08-060514.png)

举例来说，如果你下达了 `man null` 时，会出现的第一行是：`NULL(4)`，对照一下上面的数字意义， 嘿嘿！原来 null 这个玩意儿竟然是一个 **装置文件** 呢！很容易了解了吧！

> 上表中的1, 5, 8这三个号码特别重要，也请读者要将这三个数字所代表的意义背下来喔！

## 阅读 man page 的心得

1. 先察看NAME的项目，约略看一下这个数据的意思；
2. 再详看一下DESCRIPTION，这个部分会提到很多相关的数据与使用时机，从这个地方可以学到很多小细节呢；
3. 而如果这个命令其实很熟悉了(例如上面的date)，那么主要就是查询关于OPTIONS的部分了！ 可以知道每个选项的意义，这样就可以下达比较细部的命令内容呢！
4. 最后，会再看一下，跟这个数据有关的还有哪些东西可以使用的？举例来说，上面的SEE ALSO就告知我们还可以利用 `info coreutils date` 来进一步查阅数据；
5. 某些说明内容还会列举有关的文件(FILES 部分)来提供我们参考！这些都是很有帮助的！

另外，加入忘记了指令的完整名称，只记得该命令的部分关键词，该如何去查到所需要的 man page 呢？

```
[zodas@localhost ~]$ man -f keyword <== 列出命令名中含有 keyword 的所有命令
[zodas@localhost ~]$ man -k keyword <== 列出 man page 中包含 keyword 的所有命令
```

## 参考文献

- [Linux系統的線上求助man page與info page，by 鸟叔](http://linux.vbird.org/linux_basic/0160startlinux.php#manual)

