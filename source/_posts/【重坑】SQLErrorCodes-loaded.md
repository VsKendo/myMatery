---
title: 【重坑】SQLErrorCodes loaded
date: 2020-11-06 05:46:58
tags: [数据库, sql, linux]
author: VsKendo
categories: 运维
summary: 解决Linux/Mac连接数据库时出现SQLErrorCodes loaded。
description:  解决Linux/Mac连接数据库时出现SQLErrorCodes loaded。
---

# 情况

使用mysql 5.6版本
报错：异常 SQLErrorCodes loaded: [DB2, Derby, H2, HSQL, Informix, MS-SQL, MySQL, Oracle, PostgreSQL, Sybase]

通常来讲，这是我们的sql语句写错了（比如mybatis的mapper中的sql语句有错误）

但奇怪的是，我linux和windows都是同样版本的mysql，跑了同一份.sql文件。在windows平台的mysql下完美运行，而连接linux的mysql数据库就不行！

所以可能是权限设置问题（一般出现在Linux平台等对权限有要求的地方）

 这个情况在windows平台下并不常见，因为Windows对权限并没有严格要求。

# 原因

一般是由于root用户对全局host无访问权限。因此只要给root用户添加一个访问权限即可。 

# 解决方案

就我目前掌握的情况来看，出现这个问题的原因有很多，我遇到的是这两种情况，也是比较常见的。
    1.数据库的字段和输入的数据库的数据类型不匹配
比如说，一个字段int，你设置的长度是5，但是你输入了一个长度为6的值，就会出现这个错误。
     解决方法：

```bash
找到有问题的字段，并加以改正
```

2.权限设置问题（一般出现在Linux平台等对权限有要求的地方）
 这个情况在windows平台下并不常见，因为Windows对权限并没有严格要求。
 原因：一般是由于root用户对全局host无访问权限。因此只要给root用户添加一个访问权限即可。
 解决方案(此方案由网友提出，我使用后并未解决问题，正在尝试其他方法):

```bash
登陆mysql ，执行

mysql -u root -pPasswd

mysql >grant all privileges on *.* to root@"%" identified by "Passwd";

mysql >flush privileges;
```

# 参考

https://cloud.tencent.com/developer/article/1142524
