---
title: '@Git 命令详解: checkout reset 与 revert'
comments: true
categories: languages
tags: git
abbrlink: 10b5a0f5
date: 2018-01-26 16:57:09
updated: 2018-01-26 17:13:05
keywords:
description:
---

checkout 和 reset 均有两个作用域：commit 粒度、文件粒度，而 revert 只有 commit 粒度的作用域；Revert 撤销一个提交的同时会创建一个新的提交。checkout 操作的是 全局HEAD 指针，reset 操作的是 分支的HEAD 指针。

<!--more-->

## git reset

> git reset --soft HEAD~1 只修改指针的指向，不改变暂存区和工作目录的内容
> git reset --mixed HEAD~1 修改指针的指向，同时更新暂存区的内容与指针指向的版本同步，即使暂存区中有未 commit 的内容，也会被覆盖
> git reset --hard HEAD~1 修改指针的指向，同时更新暂存区的内容，并丢弃工作目录为提交的更改

### commit 粒度

- 撤销本地仓库 `hotfix` 最近一次的提交，然后将当前工作目录的内容作为新的提交覆盖那两次提交。

```shell
# 切换到 hotfix 分支
git checkout hotfix
# 将 分支的指针 指向前一次提交，并将这个新指向的版本，同步到暂存区
git reset HEAD~1（即 git reset --mixed HEAD~1）
# 将工作目录新的内容添加到本地仓库
git add .
git commit -m '作废上一次的提交，使用新的内容'
```

- 撤销本地仓库的 `hotfix` 最近一次的提交，并不保留本地工作目录未提交的内容

```
git checkout hotfix
git reset --hard HEAD~1
```

- 撤销本地仓库的 `hotfix` 最近一次提交，但保留暂存区和工作目录中的内容

```
git checkout hotfix
git reset --soft HEAD~1
```

### file 粒度

场景：

- 我在当前分支，修改了文件 `test.txt`，不小心将该文件 `add` 到了暂存区，怎样将该文件从暂存区中移除？
- 本地仓库对于文件 A 有三个版本 `version1`, `version2`, `version3`，我想在第四次提交时，使用文件 A 的 `version2`，如何解决？

通过 `git reset filename` 将文件加载到暂存区，然后重新 commit 即可。

```shell
# 使用该命令，找到 `second` 那次提交的 SHA-1，比如是：3d825f
git log
# 然后使用 git cat-file 等底层命令 找到 test.txt 的 SHA-1，比如是：6b42be
# 将该文件的这个版本，加载到暂存区
git reset 6b42be
# 或者将上一次提交的那个版本加载到暂存区
git reset HEAD~1 test.txt
# 然后重新提交
git commit
```

## git checkout
### commit 粒度

`git checkout` 会切换 全局HEAD 到不同的分支/指定的 commit id，并且更新当前的 working directory 去匹配。因为会覆盖当前的本地更改，所以更换分支前git强制你彻底放弃或者提交存储当前的更改。不同于 git reset, git checkout 不会废弃任何分支或提交，且改粒度的 checkout，git 会提示你去解决冲突。

```shell
# 切换到分支 branch1
git checkout branch1
# 从当前分支切出并创建一个新的分支 newbranch
git checkout -b new branch
# 切换到指定 commit id， 比如你想切换到 e14529 那次提交
git checkout e14529
```

### file 粒度

`git checkout filename` 与 `git reset` 类似，只是前者会直接覆盖 working directroy 的相同文件，而后者只会同步到暂存区不会改变工作目录的内容。

比如：你想丢弃本地工作目录的 `test.txt`，然后使用本地仓库上一次提交的那个版本？

```shell
git checkout  HEAD~1 test.txt
```

## git revert

`git revert` 撤销一个提交的同时会创建一个新的提交。这是一个安全的方法，因为它不会重写提交历史。比如，下面的命令会找出倒数第二个提交，然后创建一个新的提交来撤销这些更改，然后把这个提交加入项目中。

```shell
git checkout hotfix
git revert HEAD~2
```

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-01-26-084229.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-01-26-084244.png)

## 总结

git reset 和 git checkout 一般针对个人分支，git revert 一般针对公共分支，因为后者的撤销操作是会保留之前的提交历史的。

| command | 作用域 | 应用场景 |
| :-: | :-: | :-: |
|  git reset | commit 粒度 | 在私有分支上撤销一些已经 commit 的内容 |
|  git reset | file 粒度 | 将文件的制定版本加载到暂存区，如暂存区中存在该文件，则将会被覆盖 |
| git checkout | commit 粒度 | 切换分支或查看旧版本 |
|  git checkout | file 粒度 | 舍弃工作目录中的更改（直接覆盖本地的更改） |
|  git revert | commit 粒度 | 在公共分支上回滚更改 |
| git revert |  无 | 无 |


## 参考链接

- [Resetting, Checking Out & Reverting](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)
- [git reset, git checkout, git revert 区别 (译)](https://segmentfault.com/a/1190000003102737)
- [Git-工具-重置揭密](https://git-scm.com/book/zh/v2/Git-工具-重置揭密#_git_reset)


## 附图

![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-01-26-064335.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-01-26-064408.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-01-26-064655.png)
![](http://ipic-markdown.oss-cn-shanghai.aliyuncs.com/blog/2018-01-26-064723.png)

tips: Checkout and reset are generally used for making local or private 'undos'.Revert is considered a safe operation for 'public undos'.
