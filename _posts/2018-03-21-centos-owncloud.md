---
layout: post
title: 基于lnmp环境下搭建owncloud
categories: Centos
description: 基于lnmp环境下搭建owncloud
keywords: Centos owncloud
---

#### 下载owncloud

```shell
wget https://download.owncloud.org/community/owncloud-10.0.3.zip
```

#### 解压

```shell
unzip wordpress+版本号
```

#### 移动文件到你统一放置软件的位置

```shell
mv owncloud /home/xxx
```

#### 配置Nginx配置

```shell
server {
    listen 80 ;
        #listen [::]:80 default_server ipv6only=on;
        server_name www.xxx.com;  --你的域名
        index index.html index.htm index.php;
        root  /home/xxx; --你的owncloud目录

        location /wordpress {
             try_files $uri $uri/ /index.php$is_args$args;
        }



        location ~ [^/]\.php(/|$) {
            # comment try_files $uri =404; to enable pathinfo
            try_files $uri =404;
            fastcgi_pass unix:/tmp/php-cgi.sock;
            fastcgi_index index.php;
            include fastcgi.conf;
            #include pathinfo.conf;
            fastcgi_param PHP_ADMIN_VALUE "open_basedir = /home/wwwroot: /temp/:/proc";
        }       


        access_log  /home/xxx; --日志目录
}

（此配置文件只是nginx配置文件的一部分，按需引入，不懂nginx配置的百度）
```
