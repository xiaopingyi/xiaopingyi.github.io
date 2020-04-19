---
layout: post
title: Centos7搭建邮件服务器(postfix devocot cyrus-sasl)
categories: 转载
description: Centos7搭建邮件服务器(postfix devocot cyrus-sasl)
keywords: Centos mail
---


#### 基础知识了解

```shell
Dovecot :一个开源的 IMAP 和 POP3 邮件服务器
POP / IMAP 是 MUA 从邮件服务器中读取邮件时使用的协议
POP3 是从邮件服务器中下载邮件存起来
IMAP4 则是将邮件留在服务器端直接对邮件进行管理、操作
Postfix 基于GPL协议之下开发的MTA（邮件传输代理）软件
```

```shell
graph TD
    A[邮箱服务器] -->B(Dovecot)
    A-->C(postfix)
    A-->D(sasl2)
    B-->E(作为接收邮件服务器)
    C-->F(作为发送邮件服务器)
    D-->H(协议验证)
```

#### 设置域名解析

```shell
1. 首先要有指定 ip 的 A 记录解析 @ A 111.111.111.111
2. 需要有 mail 二级域名的 A 记录解析 mail A 111.111.111.111
3. MX 记录解析 @ MX mail.yijiebuyi.cn.
4. TXT解析 @ TXT v=spf1 include:spf.mail.yijiebuyi.com ~all
```

#### 停止自带mail服务

```shell
//停止 sendmail 服务
# /etc/init.d/sendmail stop

//卸载 sendmail 服务
# yum remove sendmail
```

#### 安装 postfix 和 dovecot

```shell
# yum install postfix   dovecot
```

#### 安装 cycus-sasl

```shell
# yum install cyrus-sasl-*
```

#### 配置 postfix

```shell
#  vim /etc/postfix/main.cf

myhostname=mail.yijiebuy.com  //这里要换成你自己的邮箱服务器
mydomain=yijiebuyi.com            //这里换成你自己的主机服务器
myorigin = $mydomain
inet_interfaces = all   ＃可以接收所有域名的邮件
inet_protocols = ipv4
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain,mail.$mydomain, www.$mydomain, ftp.$mydomain
local_recipient_maps =
mynetworks =0.0.0.0/0   ＃设置内网ip
relay_domains = $mydestination
home_mailbox = Maildir/

smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
broken_sasl_auth_clients = yes
smtpd_recipient_restrictions = permit_sasl_authenticated,reject_unauth_destination,permit_mynetworks
smtpd_client_restrictions = permit_sasl_authenticated
```

#### 配置 dovecot

```shell
# vim /etc/dovecot/dovecot.conf
protocols = imap pop3 lmtp
listen = *, ::

# vim /etc/dovecot/conf.d/10-auth.conf
disable_plaintext_auth = no
auth_mechanisms = plain

# vim /etc/dovecot/conf.d/10-mail.conf
mail_location = maildir:~/Maildir

# vim /etc/dovecot/conf.d/10-ssl.conf
ssl = no
```

#### 配置 sasl2

```shell
#vim /etc/sysconfig/saslauthd

MECH=shadow  #指定以本地系统用户名认证

# vim /usr/lib64/sas12/smtpd.conf    //64位系统
# vim /usr/lib/sas12/smtpd.conf       //32位系统

pwcheck_method: saslauthd
mech_list: PLAIN LOGIN
log_level:3
```

#### 启动服务

```shell
# service postfix start
# service dovecot start
# service saslauthd start

或者

# systemctl  start  dovecot
# systemctl  start  postfix
# systemctl  start  saslauthd


//如何查看 saslauthd 命令是否启动服务成功,使用 status 来查看.

systemctl status postfix
```

#### 测试发送

```shell
# echo  "hello,world" | mail -s "title" qqNumber@qq.com
```

#### 创建邮箱用户

```shell
# useradd -g groupname username -s /bin/false   //-s为默认shell，不给shell，也就不能登录
# useradd -g groupname username -s /sbin/nologin    //-s为默认shell，默认给予shell，但是不给登录shell

```

#### 重要

```shell
1.VPS一定要确保打开了25端口和110端口
2.如果使用腾讯云VPS一定要将25号端口加入到安全策略中
3.之前搭建一直没有成功，这里需要修改服务器主机名称

# hostname  ---查看当前名称
# hostname set-hostname mail.yixiaoping.me


//另外这里测试修改了 /etc/hosts下面的localhost为mail.yixiaoping.me
```
