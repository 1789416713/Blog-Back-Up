
---
title: Linux day 01
<!-- tags: 
  - Linux -->
categories:
  - Linux
abbrlink: 43567
---


记录 Linux 日常学习
<!-- more --> 

## 根目录下的常见目录

- /bin: binart 二进制文件 可执行命令  shell命令
- /dev: device Linux下 一切皆文件(硬盘、显卡、、)
- /lib: Linux运行的时候需要加载的动态库
- /mnt:手动挂载目录
- /media:
- /media: 外设的自动挂载目录
- /root: linux 的超级用户家目录
- /usr: unix system resource
    - 用户安装的应用程序 /usr/local
- /etc: 存放配置文件
    - /usr/passwd:存放用户信息
    - /usr/group: 查看都有哪些用户组
- /opt: 安装第三方应用程序
- /home: Linux所有用户的家目录
- /tmp: 存放临时文件

## 多行追加

```sh
cat >> old.txt <<EOF
heredoc> i am linux
heredoc> EOF
```

## 特殊符号

- \>或1> 输出重定向 : 把前面输出的东西输入到后面的文件中 会清除文件原有内容
- \>>或1>> 追加重定向 : 把前面输出的东西追加到后面的文件的尾部 不会清楚文件原有内容
- 0<或< 输入重定向 : 输入重定向用于改变命令的输入 后面指定输入内容 前面跟文件名
- 0<<或<< 追加输入重定向 : 后跟字符串 用来表示 输入结束 也可用ctrl+d 来结束输入
- 2> 错误重定向 : 把错误信息输入到后面的文件中 会删除文件原有内容
- 2>> 错误追加重定向 把错误信息追加到后面的文件中 不会删除文件原有内容

`2>&1 标准正常输出和标准错误输出一样 1放到那 2就放到哪`

## find简单使用

find命令用来在指定目录下查找文件。

例

```sh
# 使用 find 删除
find /data -type f -name "zhang.txt" -exec rm {} \;
```

```sh
# xargs 读取输入数据重新格式化后输出
find /daata -type f -name "*.txt" | xargs rm -rf
```

```sh
# -mtime 时间,按照修改时间查找 +7 7天以前, 7 第7天, -7 最近7天
find /log -type -f -name "*.log" -mtime +7 | xargs rm -f # 查找/log目录 删除修改日期在15天以前修改过的文件
```

```sh
# find 查找文件并使用mv移动
mv `find /data -type -f -name "*.txt"` /log
```

## grep

过滤需要的内容 并把匹配行打印出来

例

```sh
# 输出除之外的所有行 -v 选项
grep -v "zhang" file_name
```

```sh
# 在多个文件中查找
grep "zhang" file_1 file_2 file_3 .
```

## 别名

`alias` 用来设置别名 可以将一些较长的命令进行简化，使用alias时 用户必须用单引号将原有命令引起来 防止特殊符号导致错误

`alias` 命令的作用仅局限于该次登陆的操作 若每次登陆都能使用这些命令的命名 则可以将相应的alias命令存放到 bash的初始化文件中

直接输入 `alias` 会列出系统中所有已经定义的命令别名

要删除一个别名可以使用 `unalias` 命令 如:unalias l

例

```sh
# 重新定义ls命令 只需要输入l就可以列出目录
alias l=`ls -lsh`
```

## sed

流编辑器实现对文件的增删改查

处理时 把当前处理的行存储在临时的缓冲区中 称之为`模式空间` 接着用sed命令处理缓冲区的内容 处理完成之后吧缓冲区的内容送往屏幕 接着处理下一行 这样不断重复 直到文件末尾。文件内容没有改变 除非使用重定向存储输出 sed主要用来自动编辑一个或多个文件 简化对文件的反复按操作 编写转换程序等

例

查看一个文件(100行)内第20行到第30行的内容

```sh
# 生成文件
# seq == sequence 序列
seq 100 > zhang.txt
```

```sh
# -n 取消默认输出 p表示打印行
sed -n '20,30'p zhang.txt
```
