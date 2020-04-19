---
layout: post
title: Google hacker
categories: Hacker
description: Google hacker
keywords: Google hacker
---
```javascript
1. “Login: ” “password =” filetype: xls ( 搜索存储在excel文件中含有password的数据)。
2. allinurl: auth_user_file.txt (搜索包含在服务器上的 auth_user_file.txt 的文件）。
3. filetype: xls inurl: “password.xls” (查找 用户名和密码以excel格式）这个命令可以变为“admin.xls”.
4. intitle: login password (获取登陆页面的连接，登陆关键词在标题中。)
5. intitle: “Index of” master.passwd (密码页面索引)
6. index of / backup ( 搜索服务器上的备份文件）
7. intitle: index.of people.lst (包含people.list的网页）
8. intitle: index.of passwd.bak ( 密码备份文件)
9. intitle: “Index of” pwd.db (搜索数据库密码文件).
10. intitle: “Index of .. etc” passwd (安装密码建立页面索引）.
11. index.of passlist.txt (以纯文本的形式加载包含passlist.txt的页面).
12. index.of.secret (显示包含机密的文档，.gov类型的网站除外) 还可以使用: index.of.private
13. filetype: xls username password email (查找表格中含有username和password的列的xls文件).
14. .”# PhpMyAdmin MySQL-Dump” filetype: txt (列出包含敏感数据的基于php的页面)

15. inurl: ipsec.secrets-history-bugs (包含只有超级用户才有的敏感数据). 还有一种旧的用法 inurl: ipsec.secrets “holds shared secrets”

16. inurl: ipsec.conf-intitle: manpage

17. inurl: “wvdial.conf” intext: “password” (显示包含电话号码，用户名和密码的连接。）

18. inurl: “user.xls” intext: “password” (显示用户名和密码存储在xls的链接。)
19. filetype: ldb admin (web服务器查找存储在数据库中没有呗googledork删去的密码。）

20. inurl: search / admin.php (查找admin登陆的php页面). 如果幸运的话，还可以找到一个管理员配置界面创建一个新用户。
21. inurl: password.log filetype:log (查找特定链接的日志文件。)

22. filetype: reg HKEY_CURRENT_USER username (在HCU (Hkey_Current_User)路径中查找注册表文件(registyry)。)

其实，还有很多命令可以用来抓取密码或者搜素机密信息。
23. “Http://username: password @ www …” filetype: bak inurl: “htaccess | passwd | shadow | ht users”

(这条命令可以找到备份文件中的用户名和密码。)
24. filetype:ini ws_ftp pwd (通过ws_ftp.ini 文件查找admin用户的密码)

25. intitle: “Index of” pwd.db (查找加密的用户名和密码）

26. inurl:admin inurl:backup intitle:index.of (查找关键词包含admin和backup的目录。)

27. “Index of/” “Parent Directory” “WS _ FTP.ini” filetype:ini WS _ FTP PWD (WS_FTP 配置文件， 可以获取FTP服务器的进入权限)

ext:pwd inurl:(service|authors|administrators|users) “# -FrontPage-”

28. filetype: sql ( “passwd values *” |” password values *” | “pass values **“) 查找存储在数据库中的sql代码和密码。 )
```
