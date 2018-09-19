
---
title: rewrite重定向
categories:
  - Linux
abbrlink: 5
---
rewrite的组要功能是实现RUL地址的重定向
<!-- more -->

## rewrite 应用场景

Nginx的rewrite功能在企业里应用非常广泛：

- 可以调整用户浏览的URL，看起来更规范，合乎开发及产品人员的需求。
- 为了让搜索引擎搜录网站内容及用户体验更好，企业会将动态URL地址伪装成静态地址提供服务。
- 网址换新域名后，让旧的访问跳转到新的域名上。例如，访问京东的360buy.com会跳转到jd.com
- 根据特殊变量、目录、客户端的信息进行URL调整等

## 配置 rewrite

### 创建rewrite文件

```sh
# vim 编辑虚拟主机配置文件
vim conf.d/www.xxx.com.conf
```

文件内容

```sh
server {

        listen 80;
        server_name abc.com www.abc.com;

        if ( $host != 'www.abc.com'  ) {

                rewrite ^/(.*) http://www.abc.com/$1 permanent;
        }
        location / {

                root /data/www/www;
                index index.html index.htm;
        }
        error_log    logs/error_www.abc.com.log error;
        access_log    logs/access_www.abc.com.log    main;
}
```

### 重启服务

确认无误后即可重启

```sh
# 检测语法是否正确
nginx -t
# 如果无误 重启nginx
sudo systemctl restart nginx
```

### 查看跳转效果

打开浏览器访问abc.com

页面打开后,URL地址栏abc.com编程了www.abc.com说明url重写成功