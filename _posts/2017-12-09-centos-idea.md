---
layout: post
title: Centos7搭建Idea激活服务器
categories: Centos
description: Centos7搭建Idea激活服务器
keywords: Centos Idea
---

#### 下载激活程序

```shell
wget https://coding.net/u/ilanyu/p/IntelliJIDEALicenseServer/git/raw/master/IntelliJIDEALicenseServer_linux_amd64
```

#### 设置IntelliJIDEALicenseServer_linux_amd64可执行权限

```shell
chmod +x  ./IntelliJIDEALicenseServer_linux_amd64
```

#### 添加防火墙规则（这里以iptables为例）

```shell
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport xxxx -j ACCEPT
```

#### 服务命令说明

```shell
# 运行注册服务
# -p 配置端口号
# -u 配置注册用户名
# IdeaServer 为脚本名称
./IdeaServer -p 1017 -u yihy
```

#### 开机启动

```shell
vim /etc/rc.d/rc.local
### 在最后面加入以下内容
cd /home/wwwroot/            --你的执行脚本目录
screen -dmS IdeaServer ./IdeaServer -p 1017 -u yihy --第一个IdeaServer表示系统建立服务的名字，第二是你执行脚本的名称
```

#### 配置Nginx方向代理

```shell
### 安装完成的服务只能本地访问
  1.配置upstream
  upstream idea.abc.com {
                 server 127.0.0.1:1234;  --你的ideaServer地址
         }
  2.配置服务
  server {
        listen       80;
        server_name  idea.abc.com;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass   http://idea.abc.com;
            index  index.html index.htm;
        }
    }
```
