---
title: CentOS升级到CentOS Stream
date: 2022-03-19 19:48:46
tags: [CentOS, CentOs Stream, linux]
author: VsKendo
categories: 运维
summary: 从CentOS升级到CentOS Stream的方法以及错误解决方案
description: 从CentOS升级到CentOS Stream的方法以及错误解决方案
---

# 步骤

升级过程中，通常是先查看仓库是否有centos-release-stream仓库

```shell
dnf search centos-release-stream
```

再试图安装centos-release-stream仓库

```shell
dnf install -y centos-release-stream
```

这是通常会出现ERROR，导致无法升级成CentOS Stream。错误信息大概如下：

```shell
conflicts with centos-repos(8) provided by centos-stream-repos-8-2.el8.noarch
```

# 解决错误

如果你出现了这个错误，此时你使用 `yum update`应该也会出现这个错误。这是因为CentOS和CentOS Stream**仓库冲突**了。

所以**将仓库切换为CentOS Stream的仓库**即可。

```shell
dnf swap centos-linux-repos centos-stream-repos
```

再安装

```shell
dnf install centos-release-stream
```

检查是否成功

```shell
cat /etc/centos-release
```

# 后话

这个好像是官方故意引发的冲突，是为了防止运维使用了不同的仓库中下载的包，然后出现各种奇奇怪怪的问题。更新后记得 `yum update`一下，使用最新的软件保证安全性。

# References

https://blog.csdn.net/weixin_66597089/article/details/122637503
