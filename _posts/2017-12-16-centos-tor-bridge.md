---
layout: post
title: Centos7搭建利用obfs4混淆的Tor bridge
categories: Centos
description: Centos7搭建利用obfs4混淆的Tor bridge
keywords: Centos Tor
---
#### 1、安装Tor

```shell
yum install tor -y
```

#### 2、编译安装obfs4

```shell
yum install git mercurial golang -y
```

#### 开始安装

```shell
export GOPATH=`mktemp -d`
go get git.torproject.org/pluggable-transports/obfs4.git/obfs4proxy
cp $GOPATH/bin/obfs4proxy /usr/local/bin/
```

#### 3、配置Tor Bridges

首先，确认服务器上的时钟日期是正确的。

然后编辑/etc/tor/torrc，定义一个ORPort，不作为出口节点，设置成Bridge：

```shell
Log notice file /var/log/tor/notices.log --日志目录
RunAsDaemon 1
ORPort 443   --设置节点
Exitpolicy reject *:*  --这个很重要，这个是禁止Tor作为出口节点，否则流量烧的飞快。
BridgeRelay 1  --指定Tor为Bridge节点
ServerTransportPlugin obfs4 exec /usr/local/bin/obfs4proxy
ExtORPort auto
PublishServerDescriptor 0
HiddenServiceDir /var/lib/tor/hidden_service/  --生成得private key位置 里面包含hostname和private key
HiddenServicePort 80 127.0.0.1:80 --指定需要代理得地址和端口
```

#### 重启Tor

```shell
service tor restart
```

#### 4、使用网桥

查看日志文件tail -F /var/log/tor/notices.log，当看到有类似的输出，证明很成功：

```shell
[notice] Registered server transport 'obfs4' at '[::]:46396'
[notice] Tor has successfully opened a circuit. Looks like client functionality is working.
[notice] Bootstrapped 100%: Done
[notice] Now checking whether ORPort &lt;redacted&gt;:443 is reachable... (this may take up to 20 minutes -- look for log messages indicating success)
[notice] Self-testing indicates your ORPort is reachable from the outside. Excellent.
```

<img src="http://www.yixiaoping.me/wp-content/uploads/2018/04/tor7-1024x252.png" alt="" />

启动tor浏览器 ==》配置 ==》封锁和审查选择是==》输入自定义网桥中继（ip+端口（配置文件里面的端口） ）==》本地代理访问选择是==》类型socks5 地址 127.0.0.1 端口1080

#### 中途遇到问题

安装成功并且所有配置都正确但是就是没办法启动tor后来发现手动创建了log文件 解决办法删除log文件即可
