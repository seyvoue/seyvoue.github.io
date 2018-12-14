---
title: '@Install IPython on Mac Sierra(OS10.12.6)'
comments: true
categories: manual
abbrlink: e8867155
date: 2017-08-15 15:08:39
updated: 2017-08-21 17:07:29
tags: python
keywords:
description:
---

> 安装 IPython 之前，计算机中得预先安装 python 以及 PIP，本文省略这部分的安装，只讨论在 Sierra 下安装 IPython 时，你可能会遇到的问题。

<!-- more -->

## 安装 IPython 时可能会遇到的问题(如果你使用的是 python3以下的版本)

macOS 早期的版本，你只需要执行 `pip install ipyhton` 或者 `sudo install ipython` 即可完成安装，但在 macOS10 及其以上的系统上，你会发现执行上面的命令，并不能完成安装，会报错：


```shell
➜  ~ pip install ipython
Collecting ipython
  Downloading ipython-5.4.1-py2-none-any.whl (757kB)
    100% |████████████████████████████████| 757kB 1.1MB/s
Collecting prompt-toolkit<2.0.0,>=1.0.4 (from ipython)
  Downloading prompt_toolkit-1.0.15-py2-none-any.whl (247kB)
    100% |████████████████████████████████| 256kB 649kB/s
Collecting decorator (from ipython)
  Downloading decorator-4.1.2-py2.py3-none-any.whl
Requirement already satisfied: setuptools>=18.5 in /System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python (from ipython)
Collecting pickleshare (from ipython)
  Downloading pickleshare-0.7.4-py2.py3-none-any.whl
Collecting pygments (from ipython)
  Downloading Pygments-2.2.0-py2.py3-none-any.whl (841kB)
    100% |████████████████████████████████| 849kB 561kB/s
Collecting pexpect; sys_platform != "win32" (from ipython)
  Downloading pexpect-4.2.1-py2.py3-none-any.whl (55kB)
    100% |████████████████████████████████| 61kB 1.2MB/s
Collecting pathlib2; python_version == "2.7" or python_version == "3.3" (from ipython)
  Downloading pathlib2-2.3.0-py2.py3-none-any.whl
Collecting backports.shutil-get-terminal-size; python_version == "2.7" (from ipython)
  Downloading backports.shutil_get_terminal_size-1.0.0-py2.py3-none-any.whl
Collecting simplegeneric>0.8 (from ipython)
  Downloading simplegeneric-0.8.1.zip
Collecting traitlets>=4.2 (from ipython)
  Downloading traitlets-4.3.2-py2.py3-none-any.whl (74kB)
    100% |████████████████████████████████| 81kB 1.9MB/s
Collecting appnope; sys_platform == "darwin" (from ipython)
  Downloading appnope-0.1.0-py2.py3-none-any.whl
Collecting six>=1.9.0 (from prompt-toolkit<2.0.0,>=1.0.4->ipython)
  Using cached six-1.10.0-py2.py3-none-any.whl
Collecting wcwidth (from prompt-toolkit<2.0.0,>=1.0.4->ipython)
  Downloading wcwidth-0.1.7-py2.py3-none-any.whl
Collecting ptyprocess>=0.5 (from pexpect; sys_platform != "win32"->ipython)
  Downloading ptyprocess-0.5.2-py2.py3-none-any.whl
Collecting scandir; python_version < "3.5" (from pathlib2; python_version == "2.7" or python_version == "3.3"->ipython)
  Downloading scandir-1.5.tar.gz
Collecting enum34; python_version == "2.7" (from traitlets>=4.2->ipython)
  Downloading enum34-1.1.6-py2-none-any.whl
Collecting ipython-genutils (from traitlets>=4.2->ipython)
  Downloading ipython_genutils-0.2.0-py2.py3-none-any.whl
Installing collected packages: six, wcwidth, prompt-toolkit, decorator, scandir, pathlib2, pickleshare, pygments, ptyprocess, pexpect, backports.shutil-get-terminal-size, simplegeneric, enum34, ipython-genutils, traitlets, appnope, ipython
  Found existing installation: six 1.4.1
    DEPRECATION: Uninstalling a distutils installed project (six) has been deprecated and will be removed in a future version. This is due to the fact that uninstalling a distutils project will only partially uninstall the project.
    Uninstalling six-1.4.1:
Exception:
Traceback (most recent call last):
  File "/Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg/pip/basecommand.py", line 215, in main
    status = self.run(options, args)
  File "/Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg/pip/commands/install.py", line 342, in run
    prefix=options.prefix_path,
  File "/Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg/pip/req/req_set.py", line 778, in install
    requirement.uninstall(auto_confirm=True)
  File "/Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg/pip/req/req_install.py", line 754, in uninstall
    paths_to_remove.remove(auto_confirm)
  File "/Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg/pip/req/req_uninstall.py", line 115, in remove
    renames(path, new_path)
  File "/Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg/pip/utils/__init__.py", line 267, in renames
    shutil.move(old, new)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/shutil.py", line 302, in move
    copy2(src, real_dst)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/shutil.py", line 131, in copy2
    copystat(src, dst)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/shutil.py", line 103, in copystat
    os.chflags(dst, st.st_flags)
OSError: [Errno 1] Operation not permitted: '/var/folders/1t/k5ry6mpn0gj6982jgm8_hm480000gn/T/pip-_p6xLF-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/six-1.4.1-py2.7.egg-info'
```

查阅相关资料，了解到这与 OSX SIP 机制有关，网上有提到可通过取消 SIP 机制解决这个问题(比如[这篇文章](http://xiaorui.cc/2016/03/27/%E8%A7%A3%E5%86%B3mac-osx%E4%B8%8Bpip%E5%AE%89%E8%A3%85ipython%E6%9D%83%E9%99%90%E7%9A%84%E9%97%AE%E9%A2%98/))。但个人不是很喜欢去碰系统权限，于是找到了下面这个方案。

## 解决方案
基于用户的权限来安装模块包显得更加合理。
```shell
pip install ipython --user -U
```

## 验证 ipython 是否安装成功
终端执行 `python -m ipython`，终端返回以下内容，表示安装成功。

```python
➜  ~ python -m IPython
Python 2.7.10 (default, Feb  7 2017, 00:08:15)
Type "copyright", "credits" or "license" for more information.

IPython 5.4.1 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: 
```

**NOTES: ** 若你使用的是 `zsh shell`，在 `.zshrc` 配置文件中，加入以下内容后，之后只需输入`ipython`便可运行 ipython 了。

```
alias ipython='python -m IPython'
```


