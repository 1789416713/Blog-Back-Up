
---
title: Docker 使用
<!-- tags: 
  - Linux -->
categories:
  - docker
abbrlink: 44576
---

![](http://wx4.sinaimg.cn/mw690/005Jjo4tly1fv6063u8fmj30ey09kt8w.jpg)

<!-- more -->

## docker基本操作

### docker查看实时日志

```sh
docker logs -f -t --since="2017-05-31" --tail=10 edu_web_1
--since :
```

此参数指定了输出日志开始日期，即只输出指定日期之后的日志。

- -f : 查看实时日志
- -t : 查看日志产生的日期
- -tail=10 : 查看最后的10条日志。
- edu_web_1 : 容器名称

### docker run

#### 语法

`docker run [options] image [command] [arg]`

options说明：

- -a 指定标准输入类型
- -d 后台运行容器 并返回容器id
- -i 交互模式运行容器 通常与-t同时使用
- -t 为容器重新分配一个伪输入终端
- --name 为容器制定一个名称
- --dns 指定容器使用的dns服务器 默认与宿机一致
- -h 指定容器的hostname
- -e username='ritchie' 设置环境变量
- --evn-file=[] 从指定文件读入环境变量
- -m 设置容器使用内存最大值
- --link=[] 添加连接到另一个容器
- --expose=[] 开放一个端口或一组端口
- -P 随机映射一个49000~49900的端口到内部容器开放的网络端口（大写）
- -p 指定要映射的ip和端口 可以多次使用绑定多个端口 （小写）
- -v 可以把一个宿主机上的目录挂载到镜像里

#### 实例

使用docker镜像nginx:latest以后台模式启动一个容器并命名为mynginx

`docker run --name mynginx -d nginx:latest`

使用镜像nginx:latest以后台模式启动一个容器并将容器的80端口映射到主机随机端口

`docker run -d -P nginx:latest`

使用镜像nginx:latest 以后台模式启动一个容器 将容器的80端口映射到主机的80端口 主机的目录/data映射到容器的/data

`docker run -d -p 80:80 -v /data:/data nginx:latest`

使用镜像nginx:latest 以交互模式启动一个容器 在容器内执行bash命令

`docker run -it nginx:latest bash`

### docker kill 命令

杀掉一个运行中的容器

- -s 向容器发送一个信号

#### 例

`docker kill -s KILL mynginx`

### ocker rm命令

删除一个或多少容器

- -f 通过sigkill 信号强制删除一个运行中的容器
- -l 移除容器的网络连接 而非容器本身
- -v 删除与容器关联的卷

#### 例

强制删除容器db01 db02

`docker rm -f db01 db02`

移除容器nginx01对容器db01的连接 连接名db

`docker rm -l db`

删除容器nginx01并删除容器挂载的数据卷

`docker rm -v nginx01`

### Docker pause/unpause命令

暂停容器中所有进程

`docker pause`

恢复容器中所有进程

`docker upause`

#### 例

暂停数据库容器db01提供的服务

`dokcer pause db01`

### docker create 命令

创建一个新的容器但是不启动他

用法同于 docker run

#### 例

使用镜像nginx：latest创建一个容器并将容器命名为 mynginx

`docker create --name my nginx nginx:latest`

### docker exec 命令

在运行的容器中执行命令

- -d 分离模式 在后台运行
- -i 即使没有附加也保持STDIN打开
- -t 分配一个伪终端

#### 例

在容器mynginx中 以交互模式执行容器内/root/runnbot.sh脚本

`docker exec -it mynginx /bin/sh /root/runnbot.sh`

在容器mynginx中开启一个交互模式的终端

`docker exec -i -t mynginx bash`

### docker ps 命令

列出容器

- -a 显示说有容器 包括未运行的
- -f 根据条件过滤显示的内容
- -format 指定返回值的模板文件
- -l 显示最近创建的容器
- -n 列出组禁创建的n个容器
- -no-trunc 不截断输出
- -q 静默模式 只显示容器编号
- -s 显示总文件大小

#### 例

列出最近创建的5个容器

`docker ps -n 5`

### docker top 命令

查看容器中运行的进程信息 支持ps命令参数

### docker attach命令

连接正在运行的容器

### Docker events 命令

从服务器获取实时事件

- -f 根据条件过滤事件
- --since 从指定时间戳后显示所有事件
- --until 流水事件显示到指定时间为止

#### 例

显示docker镜像为mysql:5.6 1016年7月1日后相关事件

`docker events -f "image"="mysql:5.6" --since="1467302400"`

### docker logs命令

获取容器的日志

- -f 跟踪日志输出
- --since 显示某个开始时间的所有日志
- -t 显示时间戳
- --tail 仅列出最新n条容器日志

#### 例

查看mynginx的日志输出

`docker logs -f mynginx`

查看mynginx从2016年7月1日后最新10条日志

`docker logs --since="2016-7-1" --tail=10 mynginx`

### Docker export命令

将文件系统作为一个tar归档文件导出到stdout

- -o 将输出内容写到文件

#### 例

将id为b6ec967c8b1b的容器按日期保存为tar文件

`docker export -o mysql-`date +%Y%m%d`.tar b6ec967c8b1b`

### docker commit命令

从容器创建一个新的镜像

- -a 提交镜像作者
- -c 使用dockerfile来指定创建镜像
- -m 提交时说明文字
- -p 在commit时 将容器暂停

#### 例

将容器69413bc73e0f 保存为新镜像 并添加提交人信息和说明信息

`docker commit -a "myname" -m "mysql aoache" 69413bc73e0f web_3`

### docker cp 命令

用于容器与主机之间的数据拷贝

`docker cp [options] container:src_path dest_path|-`
`docker cp [options] src_path|- container:dest_path`

- -L 保持源目标的链接

#### 例

将主机/www/test 目录拷贝到容器b6ec967c8b1b中 目录重命名为www

`docker cp /www/test b6ec967c8b1b:/www`

将容器b6ec967c8b1b的/www目录拷贝到主机的/tmp

`docker cp b6ec967c8b1b:/www /tmp/`

### docker diff 命令

检查容器里文件结构更改

#### 例

查看容器web_1的文件结构更改

`docker diff web_1`

### Docker pull命令

从镜像仓库中拉取或更新指定镜像

- -a 拉去所有tagged镜像
- --disable-content-trust  忽略镜像校验 默认开启

#### 例

从Docker hub下载java最新镜像

`docker pull java`

从docker pull 下载REPOSITORY为java的所有镜像

`docker pull -a java`

### Docker push命令

将本地的镜像上传到镜像仓库 要先登陆到镜像仓库

- -disable-content-trust 忽略镜像的校验 默认开启

#### 例

上传本地镜像myapache：v1到镜像仓库中

`docker push myapache:v1`

### docker images命令

列出本地镜像

- a 列出本地所有镜像
- --digests 显示镜像的摘要信息
- -f 显示满足条件的镜像
- --format 指定返回值的模板文件
- --no-trunc 显示完整的镜像信息
- -q 只显示镜像id

#### 例

列出本地镜像中repository为nginx的镜像列表

`docker images nginx`

### docker rmi命令

删除本地一个或多个镜像

- -f 强制删除
- --no-prune 不移除该镜像的过程镜像 默认移除

#### 例

强制删除本地镜像nginx:v4

`docker rmi -f nginx:v4`

### docker tag命令

标记本地镜像 将其归入某一仓库

#### 例

将镜像nginx：v3标记为nginxv4镜像
    
`docker tag nginx：v3 nginx：v4`

### docker build 命令

使用Dockerfile创建镜像

#### 例

使用当前目录的Dockerfile 创建镜像

docker build -t nginx:v5

### docker history命令

查看指定镜像的创建历史

- -H 以可读的格式打印镜像大小和日期 默认为ture
- --no-trunc 显示完整的提交记录
- -q 仅列出提交记录id

#### 例

查看本地镜像 nginx：v3的创建历史

`docker history nginx：v3`

### docker save命令

将指定镜像保存成tar归档文件

- -o 输出到文件

#### 例

将镜像nginx：v3生成my_nginx_v3.tar文档

`docker save -o my_nginx_v3.tar  nginx:v3`

### docker import命令

从归档文件中创建镜像

- -c 应用docker指令创建镜像
- -m 提交时的说明文字

#### 例

从进行归档文件my_nginx_v3.tar创建镜像 命名为 nginx:v5

`docker import my_nginx_v3.tar nginx:v5`

### docker info命令

显示docker系统信息 包括镜像和容器数目