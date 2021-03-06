---
title: Linux day 03
<!-- tags: 
  - Linux -->
categories:
  - Linux
abbrlink: 2
---

记录 Linux 日常学习
<!-- more --> 

## linux 目录的特点

- /是所有目录的顶点
- Linux系统的所有目录是一个个有层次的倒着的树状目录结构 `/`是所有目录的起点
- 目录和磁盘分区是没有关联的
- /下不同的目录可能会对应不同的分区或磁盘
- 所有的目录按照一定的类别有规律的组织和命名

linux 里设备不挂载是看不到入口的 类似没有门的屋子 如果希望被访问 就必须给这个设备一个入口 这个入口就叫做挂载点 挂载点的表现实质就是一个目录

```sh
# 设备如果想要访问必须有挂载点
# 光驱也是一种设备
# mount /dev/cdrom /mnt
```

linux 系统中的所有目录内容按照类别组织 例如:Linux下的应用程序 他的可执行程序可能在/usr/bin 而他的数据文件和帮助在/usr/share下 运行时加载的配置文件和服务启动命令却在/etc 下

## Linux 用户管理

Liunx 是一个可以实现多用户登陆的操作系统 比如 "张三" 和 "李四" 都可以同时登陆同一台主机 他们共享主机资源 但是他们分别有自己的主机空间 用于存放各自的文件 但实际上他们的文件都是放在同一个屋里磁盘上的 甚至同一个逻辑分区或者目录里 但是用于Liunx的 用户管理 和 权限几只不同的用户不可以轻易的查看修改彼此的文件

### 查看用户

打开终端 输入命令

```sh
~ who am i
zero     ttys000  Jul 12 23:50
```

输出的第一列标书打开当前为终端的用户的用户名(要查看当前用户的用户名去掉空格直接使用 `whoami` 即可) 第二列的 `ttys000` 中的 `ttys` 表示伪终端 所谓的伪就是相对于 `/dev/tty`设备而言 伪终端就是当你在图形界面使用 /dev/ttys 时 每打卡爱一个 终端就会产生一个伪终端 `ttys000` 后面的数字就是伪终端序号 在打开一个终端 然后输入 `who am i` 就会发现 `ttys000` 变成了 `ttys001` 第三列是表示当前终端的启动时间  

### 创建用户

在Liunx系统中 `root` 账户用户系统最高权限比如添加用户

我们一般在登陆系统时使用的是普通身份登陆的 要创建用户 需要root权限 这里就需要 `sudo` 这个命令. 不过使用这个命令有两个前提,一是要知道当前登陆用户的密码,二是当前用户必须在 `sudo` 用户组

#### su,su-,sudo区别

`su <user>` 可以切换到用户user 执行时需要输入目标用户的密码,`sudo <cmd>` 可以以特级权限运行cmd命令,需要当前用户属于sudo组并且要输入当前用户密码.`su- <user>` 命令也是切换用户,同时环境变量也会跟着改变成目标用户的环境变量

现在新建一个叫 zero 的用户

```sh
# 创建用户
sudo adduser zero
# 设置密码
sudo passwd zero
```

按照提示输入密码,然后是给zero用户设置密码 这个命令不但可以添加用户到系统 同时也会默认为新用户创建home目录

```sh
ls /home
```

现在已经创建好一个用户 并且可以使用创建的用户登录了 使用如下命令切换登录用户

```sh
su -l zero
```

然后输入刚刚设置的密码 退出时可以使用 `exit` 或者使用快捷键 `ctrl+d`

### 用户组

Linux中每个用户都有一个用户组,用户组简单的理解就是一组用户的集合,他们共享资源和权限,同事拥有私有资源,就跟在家的形式差不多 你的兄弟姐妹(不用的用户) 属于同一个家(用户组) 你梦共同拥有这个家(共享资源) 父母对你们都一样(共享权限),你偶尔写写日记 其他人未经允许不准查看(私有资源和权限) 一个用户可以有多个组比如你既属于家庭 又属于公司或者学校

#### Linux中如何知道自己输入哪个组

方法一:

*使用groups命令*

```sh
~ groups zero
zero : zero docker
```

冒号之前代表用户,后面表示该用户所属的用户组,这里可以看到zero用户输入docker组 每次新建用不如果不指定组的华安 默认会自动创建一个与用户名相同的用户组.默认情况下 sudo 用户组里面可以使用sudo获取root权限, zero 用户也可以使用 sudo命令 为什么这里没有显示在sudo用户组呢？可以查看`/etc/sudoers`文件 填入后可以获得sudo权限

方法二:

查看 `/etc/group` 文件

```sh
# cat 用于读取指定文件的内容并打印到终端输出 | sort表示将读取到的文本进行排序在输出
cat /etc/group | sort
```

获得到的信息太多没关系 使用命令过滤一些不需要的信息

```sh
~ cat /etc/group | grep -E "zero"
docker:x:992:zs,ls,zero
zero:x:1002:
```

`/etc/group` 文件格式说明

里面的内容包括用户组(group)、用户组口令、GID以及该用户组所包含的用户(User) 每个用户组一条记录 格式如下

> group_name:password:GID:user_list

可以看到password字段为 `x` 并不是说他就是密码 只是表示密码不可见而已

**将其他用户加入sudo用户组**

默认情况下新建的用户是没有root权限的 也不再sudo用户组内 可以让他加入用户组并获取root权限

直接使用sudo命令会提示不在sudoers文件中 意思是zero不在sudo用户组 至于sudoers(/etc/sudoers)最好不要修改 因为操作不慎会导致比较麻烦的后果

使用usermod可以为用户添加用户组 同样你必须拥有root权限 你可以直接使用root用户为其他用户添加用户组 或者使用其他已经在sudo用户组的用户执行该命令

```sh
# 这样就添加上了
sudo usermod -G sudo zero
```

或者使用

```sh
# 修改配置文件
sudo visudo
```

再切换会zero用户就可以使用sudo权限了

### 删除用户

删除用户是件简单的事

```sh
sudo deluser zero --remove-home
```
