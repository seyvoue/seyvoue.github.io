---
title: '@Mac 终端配置策略：iterm2+oh-my-zsh+powerlevel9k'
comments: true
categories: guildlines
tags: shell
abbrlink: e5c8b56
date: 2018-12-04 15:29:53
updated:
mathjax:
keywords:
description:
---

> 本文适用于 mac 用户
> 原则：避免扰乱你的开发环境，尽可能使用 `homebrew` 来安装需要的包套件

配置完后的效果如下：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-%E6%95%88%E6%9E%9C%E5%9B%BE.png)

<!--more-->

## 安装 iterm2

step1: 使用 homebrew 安装 iterm2
```shell
# 若是第一次执行 brew cask 的话，需要先执行
brew tap caskroom/cask

# 安裝 iTerm2
brew cask instal iterm2
```

step2：修改 Report Terminal Type，以支持绚丽的配色
安装 iterm2 后，修改 `Report Terminal Type`为 `xterm-256color`：
依次`Preferences > Profiles > Terminal > Report Terminal Type`，设为`xterm-256color`

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-report%20terminal%20type.png)

## 修改 iterm2 的配色方案

设定路径：`Preferences > Profiles > Colors > Color Presets...`
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-presets.png)

内建的 color scheme 不是很好看，可以去[iTerm2 Color Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)克隆到本地，然后 import 到 iterm2 中
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-import-presets.png)

刚才克隆下来的 iTerm2-Color-Schemes 有很多文件夹，从 `schemes` 資料夾裡面選一個喜歡的 color scheme，这里我选择的是 **`Tomorrow Night Eighties`**
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-tomorrow%20night%20eighties.png)

## 安装 powerline font

由于我们要使用的 theme 会用到很多特殊的 icon，所以 iTerm2 选用的字体必须为支持这些特殊 icon 的字体。这类型的字体统称为 powerline font（另外还有加强版支持更多特殊 icon 的为 nerd font）

若沒有安装 powerline font 的话，遇到字体所不支持的 icon 时会像这样：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-non%20powerline%20font.png)

安装了 powerline font 后：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-1-0lPAd28LbancmQuHgnDyNg.png)

支持 powerline 的字体很多，这里选用的是 `Sauce Code Pro Nerd Font Complete`

step1：使用 homebrew 安装字体
```shell
# 先執行這行，才能用 homebrew 安裝字型。曾經執行過的人可以跳過這個指令
brew tap caskroom/fonts
# 安裝指令
brew cask install font-sourcecodepro-nerd-font
```

如果想要安装別的字体，brew 上面也有很多字型可以挑，关键词是 `nerd`：
```shell
brew search nerd
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-033012.png)

step2：修改字体
装完后，依次`Preferences > Profiles > Text > Change Font`，将字体改成`SauceCodePro Nerd Font`或你自己下载的字体：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-1-Br7NPgYzmLiLsalMiOXC5A.png)

### 可能出现的问题

若在切换字体后，发生 iTerm2 无法正常运作，有可能是遇到同一字体有重复版本的问题，请按一下步骤进行修改：
打开 Font Book.app -> 选择刚安装的字体 -> 选择自动解决版本问题

## 设定默认 shell 为 zsh

```
# 查看支持的 shell
cat /etc/shells
# 若没有 zsh，则安装
brew install zsh
# 将 zsh 设定为默认的 shell
chsh -s /bin/zsh
```

## 安装 oh-my-zsh
上一步装完 zsh 后，就可以开始调整我们想要的 command line 外观设定了，但是原始的 zsh 因为设定太难搞，所以多年前刚出现的时候没有受到太多关注，直到有人写了一套叫 `oh-my-zsh` 的 framework 来帮助大家使用 zsh，zsh 才火了起来。现在几乎所有 zsh 好用的工具都有支援 `oh-my-zsh`，所以当然是要装这东西。

step1：安装 oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
>注：这会直接执行 `oh-my-zsh` 的 `install.sh` 有疑虑的人可以先稍微研究一下 `oh-my-zsh` `github` 上的 `install.sh`，觉得放心再执行

执行完以后如果没有出现什么错误讯息就代表成功了，同时会发现多了 `oh-my-zsh` 的文件夹 `~/.oh-my-zsh`

## 安装 powerlevel9k 主题

刚装完 `oh-my-zsh` 以后，预设是使用内建的 theme `robbyrussell`，多了 git 资讯，颜色也看起来比原生 bash 好一些：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-1-1TqBIUz998aoEAoepG4mbw.png)

不过 `oh-my-zsh` 内建很多 `theme`，在它的 [github wiki](https://github.com/robbyrussell/oh-my-zsh/wiki/themes) 上有很多截图可以参考：


切换内建的 theme 很简单，直接修改你的 `~/.zshrc`，把原本 `ZSH_THEME=”robbyrussell”` 改成你想要的：
```shell
# 編輯 ~/.zshrc
ZSH_THEME=”agnoster” # 試試看把 robbyrussell 改成 agnoster
```
任何的 zsh 设定修改完后，还要执行以下命令才可以生效：
```shell
exec $SHELL
```
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-1-Dj2trYBv3hgFg4LOIlMtWg.png)
> `agnoster` 看起来比 `robbyrussel` 漂亮多了。

本文推荐 `powerlevel9k` 主题！
文章开头的图片就来自 [powerlevel9k 的 github](https://github.com/bhilburn/powerlevel9k)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-1-OwwhfTqbc8IUaZnCAYXt7g.gif)

`powerlevel9k` 不只是像上面的示范图显示一些基本的资讯，还可以，比如像下图那样，显示 WiFi 信号强度、笔记本剩余电量、CPU loading、system free memory 等等信息在 command line
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-1-Ixhmm4KVixyzZolr-OTV3w.png)

step1：克隆[powerlevel9k](https://github.com/bhilburn/powerlevel9k)到`~/.oh-my-zsh/custom/themes/`

```shell
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

step2：編輯`~/.zshrc` ，把 `ZSH_THEME` 设为 `powerlevel9k`
```
# nerd-font active
POWERLEVEL9K_MODE='nerdfont-complete'
ZSH_THEME="powerlevel9k/powerlevel9k"
```
**Note：**必须在`ZSH_THEME`前增加 `POWERLEVEL9K_MODE`，否则可能会出现部分 icon 无法显示。

step3：调整 command line 的提示符以及显示样式
```
# 提示符修改
# command line 左侧要显示的信息
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(dir dir_writable rbenv vcs)
# command line 右侧要显示的信息
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status root_indicator background_jobs ram load history time)
# 提示符分两行显示
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
# 在提示符与要输入的指令之间增加空格
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX="%f"
# 当前用户为 root 时，提示符为"#"，否则为"$"
local user_symbol="$"
if [[ $(print -P "%#") =~ "#" ]]; then
    user_symbol = "#"
fi
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%{%B%F{black}%K{yellow}%} $user_symbol%{%b%f%k%F{yellow}%} %{%f%}"
# 没执行完一条指令在最后增加一空行
POWERLEVEL9K_PROMPT_ADD_NEWLINE=true
```

最终的效果如下：
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-12-04-072643.png)