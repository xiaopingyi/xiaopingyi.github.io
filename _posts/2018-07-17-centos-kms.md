---
layout: post
title: 在Centos中安装KMS激活服务
categories: Centos
description: 在Centos中安装KMS激活服务
keywords: Centos KMS
---
### 获取脚本安装
```shell
wget --no-check-certificate https://github.com/teddysun/across/raw/master/kms.sh && chmod +x kms.sh && ./kms.sh
```
默认开启的端口是1688
### 常用命令

```shell
启动：/etc/init.d/kms start
停止：/etc/init.d/kms stop
重启：/etc/init.d/kms restart
状态：/etc/init.d/kms status
卸载 ./kms.sh uninstall
```

### 如何使用 KMS 服务
KMS 服务，用于在线激活 VOL 版本的 Windows 和 Office。
激活的前提是你的系统是批量授权版本，即 VL 版，一般企业版都是 VL 版。而 VL 版本的镜像一般内置 GVLK key，用于 KMS 激活。
下面列表里面含有的产品的 VL 版本或者能使用 key 进入 KMS 通道的产品，都支持使用 KMS 激活。

Office 2016：https://technet.microsoft.com/zh-cn/library/dn385360(v=office.16).aspx
Office 2013：https://technet.microsoft.com/ZH-CN/library/dn385360.aspx
Office 2010：https://technet.microsoft.com/ZH-CN/library/ee624355(v=office.14).aspx
Windows：https://docs.microsoft.com/zh-cn/windows-server/get-started/kmsclientkeys

使用管理员权限运行 cmd 查看系统版本，命令如下： wmic os get caption

使用管理员权限运行 cmd 安装从上面列表得到的 key，命令如下：
```shell
slmgr /ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
```

使用管理员权限运行 cmd 将 KMS 服务器地址设置为你自己的 IP 或 域名，命令如下：
```shell
slmgr /skms Your IP or Domain
```
注意：本脚本所做的工作就是此步骤。当你的 KMS 服务出于启动状态，那么此处就可以设置为你自己的 KMS 服务器地址。

使用管理员权限运行 cmd 手动激活系统，命令如下：
```shell
slmgr /ato
```
关于 Office 的激活，要求必须是 VOL 版本，否则无法激活。

找到你的 Office 安装目录，32 位默认一般为 C:\Program Files (x86)\Microsoft Office\Office16
64 位默认一般为 C:\Program Files\Microsoft Office\Office16
Office16 是 Office 2016，Office15 就是 Office 2013，Office14 就是 Office 2010。
打开以上所说的目录，应该有个 OSPP.VBS 文件。
使用管理员权限运行 cmd 进入 Office 目录，命令如下：

cd "C:\Program Files (x86)\Microsoft Office\Office16"
使用管理员权限运行 cmd 注册 KMS 服务器地址：

cscript ospp.vbs /sethst:Your IP or Domain
使用管理员权限运行 cmd 手动激活 Office，命令如下：

cscript ospp.vbs /act
注意： KMS 方式激活，其有效期只有 180 天。

每隔一段时间系统会自动向 KMS 服务器请求续期，请确保你自己的 KMS 服务正常运行。
