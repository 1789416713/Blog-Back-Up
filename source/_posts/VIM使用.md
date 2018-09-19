
---
title: Vim 使用

categories:
  - Vim
abbrlink: 6
---

![](http://wx4.sinaimg.cn/mw690/005Jjo4tly1fv60f6mxjyj30gk0dtaah.jpg)

主要讲解 Vim使用过程中一些指令以及注意事项.
<!-- more -->

## 安装

```sh
sudo aptet install vim //Ubuntu
```

其他平台自行百度

## 新手指南

vim最全面的教程

```sh
vimtutor //vim 教程
```

## 简单使用

下面是整理的简单归纳

### 移动光标

```vim
hjkl
2w 向前移动两个单词
3e 向前移动到第 3 个单词的末尾
0 移动到行首
$ 当前行的末尾
gg 文件第一行
G 文件最后一行
行号+G 指定行
ctrl+o 跳转回之前的位置
ctrl+i 返回跳转之前的位置
```

### 退出

```vim
esc 进入正常模式
:q! 不保存退出
:wq 保存后退出

```

### 删除

```vim
x 删除当前字符
dw 删除至当前单词末尾
de 删除至当前单词末尾，包括当前字符
d$ 删除至当前行尾
dd 删除整行
2dd 删除两行
```

### 修改

```vim
i 插入文本
A 当前行末尾添加
r 替换当前字符
o 打开新的一行并进入插入模式
```

### 撤销

u 撤销
ctrl+r 取消撤销

### 复制粘贴剪切

```vim
v 进入可视模式
y 复制
p 粘贴
yy 复制当前行
dd 剪切当前行
```

### 状态信息

ctrl+g 显示当前行以及文件信息

### 查找

```vim
/ 正向查找（n：继续查找，N：相反方向继续查找）
? 逆向查找
% 查找配对的 {，[，(
:set ic 忽略大小写
:set noic 取消忽略大小写
:set hls 匹配项高亮显示
:set is 显示部分匹配
```

### 替换

```vim
 :s/old/new 替换该行第一个匹配串
 :s/old/new/g 替换全行的匹配串
 :%s/old/new/g 替换整个文件的匹配串
```

### 执行外部命令

```vim
:!shell 执行外部命令
```

## 基本配置

### 文件编码

```vim
set encoding=utf-8
```

### 显示行号

```vim
set number
```

### 取消换行

```vim
set nowrap
```

### 显示光标当前位置

```vim
set ruler
```

### 突出显示当前行

```vim
set cursorline
```

### 主题

```vim
syntax enable
set background=dark
colorscheme solarized
```

### VIM常用命令

![VIM](http://wx4.sinaimg.cn/mw690/005Jjo4tly1fv60gszcbkj30ps0r2q3o.jpg)