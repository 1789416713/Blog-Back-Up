
---
title: npm 换源
<!-- tags: 
  - Linux -->
categories:
  - 日常
abbrlink: 4
---

![](http://wx1.sinaimg.cn/mw690/005Jjo4tly1fvcqlo8yzyj30a003w0g0.jpg)

由于npm的源在国外，所以有时候使用起来，会碰到速度极慢的情况，好在一些公司做了源的同步，可以使用这些源来达到加速下载，减少等待时间

<!-- more --> 

## npm

全称Node Package Manager，是node.js的模块依赖管理工具。由于npm的源在国外，所以 国内用户使用起来
各种不方便，下面整理出了一部分国内优秀的npm镜像资源，国内用户可以选择使用。

## 国内优秀npm镜像

### 淘宝npm源

- 搜索地址: http://npm.taobao.org/
- registry地址：http://registry.npm.taobao.org/

### cnpmjs镜像

- 搜索地址：http://cnpmjs.org/
- registry地址：http://r.cnpmjs.org/

## 如何使用 

有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法。以淘宝npm镜像距离。

1. 临时使用

`npm --registry https://registry.npm.taobao.org install express`

2. 持久使用

```sh
# 配置后可通过下面方式来验证是否成功
npm config set registry https://registry.npm.taobao.org

# 或npm info express
npm config get registry

```

3. 通过cnpm

`npm install -g cnpm --registry=https://registry.npm.taobao.org`