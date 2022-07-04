---
title: 创建mysql账户并在vscode中使用
date: 2022-07-04 20:39:57
tags: [数据库, sql, vscode]
author: VsKendo
categories: 运维
summary: 在vscode中管理数据库，又不想使用root账户。
description:  在vscode中管理数据库，又不想使用root账户。
---

# 好处

不使用root用户很关键。

# 命令

```sql
CREATE USER 'username'@'LOCALHOST' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON databaseName.* TO username@localhost ;
ALTER USER 'username'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

将其中的username、password和databaseName变成你需要的值。

# vscode中

1. 安装2个扩展：
   1. MySQL - 作者是 Jun Han
   2. MySQL Syntax - 作者是Jake Bathman
2. 打开资源管理器，在最下面找到mysql，点击加号
3. 输入localhost、用户名、密码和端口号，ssl证书可以直接回车不输入

