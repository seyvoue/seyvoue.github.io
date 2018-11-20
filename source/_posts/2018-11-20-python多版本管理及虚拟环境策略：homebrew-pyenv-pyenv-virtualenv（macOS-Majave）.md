---
title: '@python多版本管理及虚拟环境策略：homebrew + pyenv + pyenv-virtualenv（macOS Majave）'
comments: true
categories: tech
abbrlink: 77fe63cf
date: 2018-11-20 02:39:02
updated:
tags: python
keywords:
description:
---

## 概述
### 背景

- Python 解释器版本混乱, 2和3差别巨大, 而且细分版本也不尽相同, 难以选择和管理。
- 不同 Linux 发行版自带 Python 不同, 如 macOSX 自带 2.7 版本, 其中系统许多组件依赖于自带解释器, 一旦删除或者更改都可能会造成系统出问题。
- 不同的 Python 解释器软件包管理也是问题, 如 pip 和 ipython 等必备包组件, 而且在项目开发中如何保证不同的包环境互不干扰也是一个问题。

那么有没有一个终极的解决办法能在管理不同解释器版本的同时控制不同的包环境呢? 有的, 就是 `pyenv`.

<!—more—>

### pyenv 是什么? 能干什么?

> `pyenv` 是一个 forked 自 ruby 社区的简单、低调、遵循 UNIX 哲学的Python 环境管理工具, 它可以轻松切换全局解释器版本, 同时结合 `vitualenv` 插件可以方便的管理对应的包源。

使用 `pyenv` 我可以方便的下载指定版本的 python 解释器, pypy, anaconda 等, 可以随时自由的在 “shell环境、本地、全局”切换python解释器。

开发的时候不需要限定某个版本的虚拟环境, 只需要在部署的时候用 pyenv 指定某个版本就好了。

`pyenv` 切换解释器版本的时候, pip 和 ipython 以及对应的包环境都是一起切换的, 所以如果你要同时运行 ipython2.x 和 ipython3.x 多个解释器验证一些代码时就很方便。

`pyenv` 也可以创建好指定的虚拟环境, 但不需要指定具体目录, 自由度更高, 使用也简单。

### 基本原理

如果要讲解pyenv的工作原理，基本上采用一句话就可以概括，那就是：修改系统环境变量`PATH`。

对于系统环境变量`PATH`，相信大家都不陌生，里面包含了一串由冒号分隔的路径，例如`/usr/local/bin:/usr/bin:/bin`。每当在系统中执行一个命令时，例如`python`或`pip`，操作系统就会在`PATH`的所有路径中从左至右依次寻找对应的命令。因为是依次寻找，因此排在左边的路径具有更高的优先级。

而`pyenv`做的，就是在`PATH`最前面插入一个`$(pyenv root)/shims`目录。这样，`pyenv`就可以通过控制`shims`目录中的Python版本号，来灵活地切换至我们所需的Python版本。

## 如何安装？
### 安装 pyenv

本文只介绍 mac 下利用 homebrew 的安装过程，其它系统安装过程大同小异，具体可参考官方的[安装手册](https://github.com/pyenv/pyenv#installation)。

step0: preinstall

```shell
#确保本机已安装 xcode 且为最新版
[mac]xcode-select --install
#确保本机已安装相关依赖
[mac]brew install zlib openssl readline xz sqlite
```

step1: 安装 pyenv

```shell
[mac] brew install pyenv
```

step2: 在 `~/.zshrc` 添加以下内容

```
#pyenv
export PYENV_ROOT=$(brew --prefix pyenv)
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

step3: 使 `.zshrc` 生效，并重新启动 shell

```shell
[mac] source ~/.zshrc
[mac] exec $SHELL       
```

step4：验证是否安装成功

```shell
[mac]pyenv -v
pyenv 1.2.8
```

#### 可能遇到的问题
> 若遇到无法安装的问题可参考 [此文1](https://medium.com/@pimterry/setting-up-pyenv-on-os-x-with-homebrew-56c7541fd331) [此文2](https://github.com/pyenv/pyenv/wiki/Common-build-problems)，即可解决。

笔者在使用 `brew install pyenv` 后遇到以下问题：
- 问题1：
笔者在安装 pyenv 之前，mac 中已装有以下 python：
- 系统自带的 python 2.7
- homebrew 安装的 python@2 和 python3(python 3.7.1)

```shell
[mac]pyenv install -l
Available versions:
/usr/local/bin/python-build: /usr/local/bin/sort: /usr/local/opt/python3/bin/python3.6: bad interpreter: No such file or directory
```

于是，笔者便分别查看了文件`/usr/local/bin/python-build`和文件`/usr/local/bin/sort`，发现 `/usr/local/opt/python3/bin/` 中并没有 `python3.6`，只有 `python3.7`，于是手动 `ln -s [python3.7的 bin 路径] /usr/local/opt/python3/bin/python3.6`，修改后发现又出现了问题2。

- 问题2

 ```shell
[mac]pyenv install -l
Available versions:
Traceback (most recent call last):
  File "/usr/local/bin/sort", line 7, in <module>
    from sort import cli
ModuleNotFoundError: No module named 'sort'
 ```

于是，`vi /usr/local/bin/sort`:

```python
def cli():
    print('This is suroegin's package - sort')
```

没发现有什么问题，试着将`print('This is suroegin's package - sort')` -> `print("This is suroegin's package - sort")`，又出现了问题3。

- 问题3
具体的报错信息忘记了，大致包含以下关键词：
```
[mac]pyenv install -l
unmatched '
```

**最后**

```
[mac]pip3 uninstall sort
```

所有问题就解决了。

> 总结：因为笔者之前有安装 python，导致安装 pyenv 后，pyenv 的相关依赖使用了笔者系统中已安装的模块，以致出现相关依赖问题。所以，一个好的版本管理策略是多么重要，仅仅依赖 homebrew 的 `brew switch python 3.x.x` 并不能从根本上解决 python 的版本管理问题。

### 安装 pyenv-virtualenv

step1: install

```shell
[mac]brew install pyenv-virtualenv
```

step2: 修改 `~/.zshrc`为以下：

```
#pyenv
export PYENV_ROOT=$(brew --prefix pyenv)
export PATH="$PYENV_ROOT/bin:$PATH"
if which pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
if which pyenv-virtualenv-init > /dev/null; then
  eval "$(pyenv virtualenv-init -)"
fi
```

step3: 使 `.zshrc` 生效

```shell
[mac]$ source ~/.zshrc
```

## 如何使用
### pyenv 常用命令

pyenv 的主要功能如下：

```shell
$ pyenv -h
Usage: pyenv <command> [<args>]

Some useful pyenv commands are:
   commands    List all available pyenv commands
   local       Set or show the local application-specific Python version
   global      Set or show the global Python version
   shell       Set or show the shell-specific Python version
   install     Install a Python version using python-build
   uninstall   Uninstall a specific Python version
   rehash      Rehash pyenv shims (run this after installing executables)
   version     Show the current Python version and its origin
   versions    List all Python versions available to pyenv
   which       Display the full path to an executable
   whence      List all Python versions that contain the given executable

See `pyenv help <command>' for information on a specific command.
For full documentation, see: https://github.com/pyenv/pyenv#readme
```

比如：

```
# 查看当前激活的是那个版本的Python
pyenv version

# 查看所有已安装的版本
pyenv versions

# 查看所有可安装的版本
pyenv install --list

# 安装指定版本
pyenv install 3.6.5
# 安装完成后必须rehash
pyenv rehash

# 删除指定版本
pyenv uninstall 3.5.2

# 指定局部版本，当前目录生效
pyenv local 3.6.5

# 指定全局版本，整个系统生效
pyenv global 3.6.5

# 指定多个全局版本, 3版本优先
pyenv global 3.6.5 2.7.14

# 取消设置
pyenv local --unset

# 实际上当你切换版本后, 相应的pip和包仓库都是会自动切换过去的
```


### 使用 pyenv：切换 python 版本

`pyenv` 可以从三个维度来管理Python环境，简称为：`当前系统(global)`、`当前目录(local)`、`当前shell`。这三个维度的优先级从左到右依次升高，即`当前系统`的优先级最低、`当前shell`的优先级最高。

如果想修改系统全局的Python环境，可以采用 `pyenv global PYTHON_VERSION` 命令。该命令执行后会在 `$(pyenv root)` 目录中创建一个名为 `version` 的文件（如果该文件已存在，则修改该文件的内容），里面记录着系统全局的Python版本号。

```shell
[mac]$ pyenv global system
[mac]$ cat $(pyenv root)/version
system
[mac]$ pyenv version
system (set by /usr/local/opt/pyenv/version)
```

通常情况下，对于特定的项目，我们可能需要切换不同的Python环境，这个时候就可以通过`pyenv local PYTHON_VERSION` 命令来修改`当前目录`的Python环境。命令执行后，会在当前目录中生成一个`.python-version`文件（如果该文件已存在，则修改该文件的内容），里面记录着当前目录使用的Python版本号。

```shell
[mac]$ cd ~/workspace/test-pyenv
[mac]pyenv local 2.7.8
[mac]$ cat .python-version
2.7.8
[mac]$ pyenv version
2.7.8 (set by /Users/seyvoue/workspace/test-pyenv/.python-version)
[mac]$ pip -V
pip 18.1 from /usr/local/opt/pyenv/versions/2.7.8/lib/python2.7/site-packages/pip (python 2.7)
```

可以看出，`当前目录`中的`.python-version`配置优先于系统全局的`$(pyenv root)/version`配置。

另外一种情况，通过执行 `pyenv shell PYTHON_VERSION` 命令，可以修改当前shell的Python环境。执行该命令后，会在当前shell session（Terminal窗口）中创建一个名为`PYENV_VERSION` 的环境变量，然后在当前shell的任意目录中都会采用该环境变量设定的Python版本。此时，`当前系统`和`当前目录`中设定的Python版本均会被忽略。

```shell
[mac]$ cd ~/workspace/test-pyenv
[mac]pyenv local 2.7.8
[mac]$ cat .python-version
2.7.8
[mac]$ pyenv version
2.7.8 (set by /Users/seyvoue/workspace/test-pyenv/.python-version)
[mac]$ echo $PYENV_VERSION

[mac]$ pyenv shell 3.7.1
[mac]$ echo $PYENV_VERSION
3.7.1
[mac]$ cat .python-version
2.7.8
[mac]$ pyenv version
3.7.1 (set by PYENV_VERSION environment variable)
```

顾名思义，`当前shell`的Python环境仅在当前shell中生效，重新打开一个新的shell后，该环境也就失效了。如果想在`当前shell`中取消shell级别的Python环境，采用`unset`命令重置`PYENV_VERSION`环境变量即可。

```shell
cat .python-version
2.7.8
[mac]$ pyenv version
3.7.1 (set by PYENV_VERSION environment variable)
[mac]$ unset PYENV_VERSION
2.7.8 (set by /Users/seyvoue/workspace/test-pyenv/.python-version)
```

特别建议：

```
系统全局用系统默认的Python比较好，不建议直接对其操作
pyenv global system
```
```
用local进行指定版本切换，一般开发环境使用。
pyenv local 2.7.10
```
```
对当前用户的临时设定Python版本，退出后失效
pyenv shell 3.5.0
```
```
取消某版本切换
pyenv local 3.5.0 --unset
```

> 输入python即可使用新版本的python
> 系统自带的脚本会以 `/usr/bin/python` 的方式直接调用老版本的python，因而不会对系统脚本产生影响；
> 如果通过homebrew安装python，那么pip会同时安装。


### 使用 pyenv-virtualenv：管理多个依赖库环境
经过以上操作，我们在本地计算机中就可以安装多个版本的Python运行环境，并可以按照实际需求进行灵活地切换。然而，很多时候在同一个Python版本下，我们仍然希望能根据项目进行环境分离，在pyenv中，pyenv-virtualenv 插件可以实现这个功能。

使用方式如下：

``` shell
$ pyenv virtualenv PYTHON_VERSION PROJECT_NAME
```

其中，`PYTHON_VERSION`是具体的Python版本号，例如，`3.7.1`，`PROJECT_NAME`是我们自定义的项目名称。比较好的实践方式是，在`PROJECT_NAME`也带上Python的版本号，以便于识别。

现假设我们有`test-pyenv`这么一个项目，想针对`Python 2.7.8`和`Python 3.7.1`分别创建一个虚拟环境，那就可以依次执行如下命令。

```shell
$ pyenv virtualenv 3.7.1 py37_test-pyenv
$ pyenv virtualenv 2.7.8 py27_test-pyenv
```

创建完成后，通过执行`pyenv virtualenvs`命令，就可以看到本地所有的项目环境。 

```shell
$ pyenv virtualenvs 
2.7.8/envs/py27_test-pyenv (created from /usr/local/opt/pyenv/versions/2.7.8)
3.7.1/envs/py37_test-pyenv (created from /usr/local/opt/pyenv/versions/3.7.1)
py27_test-pyenv (created from /usr/local/opt/pyenv/versions/2.7.8)
py37_test-pyenv (created from /usr/local/opt/pyenv/versions/3.7.1)
```

通过这种方式，在同一个Python版本下我们也可以创建多个虚拟环境，然后在各个虚拟环境中分别维护依赖库环境。

例如，`py37_test-pyenv`虚拟环境位于`$(pyenv root)/versions/3.7.1/envs`目录下，而其依赖库位于`$(pyenv root)/versions/3.7.1/lib/python3.7/site-packages`中。

```shell
$ cd ~/workspace/test-pyenv
$ pyenv version
2.7.8 (set by /Users/seyvoue/workspace/test-pyenv/.python-version)
$ pip -V
pip 18.1 from /usr/local/opt/pyenv/versions/2.7.8/lib/python2.7/site-packages/pip (python 2.7)
```

后续在项目开发过程中，我们就可以通过`pyenv local XXX`或`pyenv activate PROJECT_NAME`命令来切换项目的Python环境。

```shell
$ cd ~/workspace/test-pyenv
$ pyenv local py37_test-pyenv
$ pyenv version
py37_test-pyenv (set by /Users/seyvoue/workspace/test-pyenv/.python-version)
$ python -V
Python 3.7.1
$ pip -V
pip 10.0.1 from /usr/local/opt/pyenv/versions/3.7.1/envs/py37_test-pyenv/lib/python3.7/site-packages/pip (python 3.7)
```

可以看出，切换环境后，pip命令对应的目录也随之改变，即始终对应着当前的Python虚拟环境。

对应的，采用`pyenv deactivate`命令退出当前项目的Python虚拟环境。

如果想移除某个项目环境，可以通过如下命令实现。

```shell
$ pyenv uninstall PROJECT_NAME
```

以上便是日常开发工作中常用的pyenv命令，基本可以满足绝大多数依赖库环境管理方面的需求。

## 参考链接

- [使用pyenv管理多个Python版本依赖环境](https://debugtalk.com/post/use-pyenv-manage-multiple-python-virtualenvs/#%E7%AE%A1%E7%90%86%E5%A4%9A%E4%B8%AA%E4%BE%9D%E8%B5%96%E5%BA%93%E7%8E%AF%E5%A2%83)
- [Python版本管理神器-pyenv](https://zhuanlan.zhihu.com/p/36402791)
- [Simple Python Version Management: pyenv, from github](https://github.com/pyenv/pyenv#pyenv-does)