---
layout: post
title: openresty搭建和相关知识
categories: Centos
description: openresty搭建和相关知识
keywords: Centos openresty
---

### 服务器环境搭建

#### 1、安装openresty

加强版的nginx 。
openresty 使用lua 语言开发各种功能
作用：
流量分发、域名绑定、负载均和、静态资源处理、waf防火墙、网关等。

1.下载依赖包

```shell
yum install readline-devel pcre-devel openssl-devel gcc perl
```

2.下载源码包：
官网 http://openresty.org/cn/

```shell
wget https://openresty.org/download/openresty-1.11.2.5.tar.gz

```

3.编译openresty

```shell
./configure --prefix=/opt/openresty \
--with-luajit \
--without-http_redis2_module \
--with-http_iconv_module

```

4.安装

```shell
gmake;
gmake install

```

<ol>
<li>设置openresty的环境变量</li>
</ol>

可以在控制台直接使用nginx命令

在 /etc/profile 文件里面加入 openresty nginx sbin目录。

```shell
vim /etc/profile

```

最后输入

```shell
nginx -V 看到openresty的版本号即可安装成功。

```

##### openresty常用命令

我们的安装目录是 */opt/openresty/nginx</code>*

```shell
nginx [-?hvVtq] [-s signal] [-c filename] [-p prefix] [-g directives]

-?,-h           : 打开帮助信息
-v              : 显示版本信息并退出
-V              : 显示版本和配置选项信息，然后退出
-t              : 检测配置文件是否有语法错误，然后退出
-q              : 在检测配置文件期间屏蔽非错误信息
-s signal       : 给一个 nginx 主进程发送信号：stop（停止）, quit（退出）, reopen（重启）, reload（重新加载配置文件）
-p prefix       : 设置前缀路径（默认是：/usr/local/Cellar/nginx/1.2.6/）
-c filename     : 设置配置文件（默认是：/usr/local/etc/nginx/nginx.conf）
-g directives   : 设置配置文件外的全局指令





pkill -9 nginx

```

##### 配置使用openresty

如果我们不在openresty安装目录下面，想要在自定义文件夹作为项目的跟目录。这时候需要使用 -p 指定目录 -c 指定配置文件

```shell
nginx -p /opt/webserver/www -c conf/nginx.conf

```

##### openresty学习资料

1、看 《openresty最佳实践》  https://moonbingbing.gitbooks.io/openresty-best-practices/content/index.html

2、openresty开发的全部api：https://github.com/iresty/nginx-lua-module-zh-wiki
