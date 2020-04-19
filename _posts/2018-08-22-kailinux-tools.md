---
layout: post
title: NETCAT ---NC
categories: Hacker
description: Kalinux netcat
keywords: Kalinux netcat
---

#### NETCAT ---NC

1. 网络工具中的瑞士军刀-小身材、大智慧

2. 侦听模式、传输模式

```shell
nc -nv 1.1.1.1 80 web服务器
nc -nv 1.1.1.1 110 pop3
nc -nv 1.1.1.1 25 smtp


聊天----
A nc -l -p 444  打开并监听端口
B nc -nv A地址 444

电子取证

---在B主机中显示A主机的当前文件夹信息
A(1.1.1.2) 待取证机子 ls -l | nc -nv 1.1.1.1 400
B（1.1.1.1） nc -l -p 400     打开本地400端口并监听


---在B主机中将A主机中所有打开的文件保存为B主机一个文件
A(1.1.1.2) lsof | nc -nv 192.168.254.128 400 -q 1
B（1.1.1.1） nc -l -p 400 &gt; lsof.txt
```

3、telnet/获取banner信息

4、传输文本信息

```shell
A：nc -lp 333 &gt;1.mp4  --服务端
B：nc -nv 444 &lt;1.mp4 -q 1 --客户端

或
A：nc -q 1 -lp 333 &lt; 1.mp4  ---服务端并传送文件
B：nc -nv 333 &gt; 1.mp4 --客户端接收文件

```

5、传输文件/目录

```shell
A：tar -cvf - abc/ | nc -lp 333 -q 1
B：nc -nv 333 | tar -xvf-
```

6、加密传输文件

7、远程控制/木马

8、加密所有流量

9、流媒体服务器

10、远程克隆硬盘
