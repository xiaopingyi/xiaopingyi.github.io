---
layout: post
title: Kalinux常用命令
categories: kali
description: Kalinux常用命令
keywords: Kalinux
---

#### 查看文件
```shellls -l    查看文件详细信息
ls -lh   显示文件大小，已kb/M/GB这种能够接受的方式
ls -a    查看隐藏文件
ls -la   查看列表文件，并显示出隐藏文件

```

#### 进入文件
```shell
cd /etc
```

#### 显示当前文件夹路径
```shell
pwd
```

#### 查看文件
```shell
cat      查看文件信息，但是显示所有信息，不方便查看
more     分页查看所有信息，方便阅读
tail     查看文件最后十行信息
tail -n 20 /var/log/message 查看最后二十行信息
watch -n 2 tail /var/log/message  每两秒刷新文件最后十行信息
```

#### 复制文件
```shell
cp abc abc1     复制abc文件为abc1
cp -r abc abc1  复制文件夹abc为abc1

```

#### 删除文件
```shell
rm abc          删除文件abc
rm -f abc       删除文件夹abc
```

#### 监控系统情况
```shell
top             查看系统信息，包括进程、磁盘、内存等
```

#### ps查看进程
```shell
ps -ef          查看进程
```

#### 筛选
```shell
grep ssh /etc/passwd    筛选出/etc/passwd目录下的包含ssh的信息行
```

#### 网络信息和关闭网卡
```shell
ifconfig               显示网卡信息
ifconfig eth0 down\up     关闭\开启eth0网卡
```

#### 修改mac地址，重启后恢复真实mac地址
```shell
macchanger eth0 00:11:11:11:11:11  修改mac地址为00:11:11:11:11:11
```

#### 查看网络连接情况
```shell
netstat -pantu | egrep -v "0.0.0.0|:::" | awk '{print $5}' | sort | egrep -v "and|Address" |cut -d ":" -f 1 &gt;ip
查看网络连接情况，排除0.0.0.0 ：：： and Address 打印第五列 排序 已冒号分隔显示第一位 输出到ip这个文件
```

#### 挂载
```shell
mount -o loop kali.iso /temp/   --挂载kali.ios到temp目录下
```

#### 文件查找

```shell
find / -name nmap    --在根目录下查找名字为nmap的文件
whereis nmap         --在系统数据库中查找nmap， whereis在系统数据库中查找 find查找所以文件
```
