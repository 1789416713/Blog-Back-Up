---
title: Git使用
tags: 
  - Git
categories:
  - 日常
abbrlink: 12
---

![](http://wx3.sinaimg.cn/mw690/005Jjo4tly1fvdzhd5da2j318g0ikmyn.jpg)

Git是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。

<!-- more --> 

## 工作流

本地的仓库由git维护的三棵树组成。 第一个是你的 `工作目录` 他拥有实际文件;第二个是 `暂存区` 他像个缓存区域,临时保存你的改动;最后是 `HEAD` 他指向你最后一次提交结果。

working dir -->(add) index -->(commit) HEAD

## 添加和提交

你可以提出更改 把他们添加到缓存区 可以使用如下命令

```sh
git add <filename>
git add *
```

这是git基本工作流程的第一步;使用如下命令以实际提交改动

```sh
git commit -m "代码提交信息"
```

现在，你的改动已经提交到了HEAD 但是还没到你的远程仓库

## 推送改动

你现在的改动已经在本地仓库的HEAD中 执行如下命令可以将改动提交到远程仓库

```sh
git push origin master
```
可以吧master换成你想要推送的分支

如果你还没有克隆现有仓库 并想讲你的仓库连接到某个远程服务器 可以使用如下命令添加：

```sh
git remote add origin <server>
```
这样你就能将你的改动推送到所添加的服务器上去了

## 分支

分支是用来将特性开发绝缘开的 在你创建仓库的时候 master是默认分支。在其他分支上进行开发 完成后再将他们合并到主分支上

创建一个叫做 feature的分支 并切换过去

```sh
git checkout -b feature
```

切换回主分支

```sh
git checkout master
```

再把新建的分支删掉

```sh
git branch -d feature
```

除非你将分支推送到远端你仓库 不然该分支就是不为他人所见的

```sh
git push origin <branch>
```

## 合并与更新

要更新你本地的仓库至最新改动:

```sh
git pull
```

以在你的工作目录获取并合并远端改动。要合并其他分支到你当前分支例如master 执行：

```sh
git merge <branch>
```

在这两种情况下 git都会尝试自动合并改动 但是这并非每次都能成功 并可能出现冲突 这时候就需要修改这些文件来手动合并这些冲突 改完之后 你需要执行如下命令将他们标记未成功：

```sh
git add <filename>
```

在合并改动之前 你可以使用如下命令预览差异

```sh
git diff <source_branch> <target_branch>
```

## 标签

为软件发布创建标签是推荐的。这个概念早就存在 在svn中也有 可以执行如下命令创建一个叫做 1.0.0的标签

```sh
git tag 1.0.0 1b2e1d63ff
```

1b2e1d63ff 是你想标记提交id的前10位字符。可以使用下列命令获取提交id

```sh
git log
```

你也可以使用少一点的提交id前几位 只要它的指向具有唯一性。

## log

如果你想要了解本地仓库的历史记录 最简单的命令就是使用:

```git
git log
```

你可以添加一些参数来修改输出 从而得到自己想要的结果。只看某一人的提交记录:

```sh
git log --author=zhang
```

一个压缩后的每一条激活只占一行的输出

```sh
git log --pretty=online
```

使用ASCII艺术的树形结构来展示所有分支 每个分支都表示了他的名字和标签

```sh
git log --graph --online --decorate --all
```

看看那些文件改变了

```sh
git log --name-status
```

具体参考 git log --help

## 替换本地改动

假如你操作失误 可以使用如下命令替换本地改动

```sh
git checkout -- <filename>
```

此命令会使用HEAD中最新的内容替换掉你工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响

假如你想丢弃你本地的修改与提交，可以到服务器上获取最新版本历史 并将你本地分支指向他

```sh
git fetch origin
git reset --hard origin/master
```
