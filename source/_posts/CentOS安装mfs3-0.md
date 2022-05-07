---
title: CentOS安装mfs3.0
date: 2020-10-14 14:23:32
tags: [CentOS, mfs]
img: /medias/featureimages/24.jpg
author: VsKendo
categories: 运维
summary: 在Linux上安装mfs系统的方法。
description: 在Linux上安装mfs系统的方法。
---

# 需求：

需要安装一个日志文件系统，于是服务器按要求装了centos6
有6块1TB的STAT hdd，还有一个300G的SAS作为系统盘。
日志要求使用nfs协议，考虑到维护、拓展等因素，决定搭建mfs，然后在这基础上使用nfs。
系统IP为内网IP：210.38.69.12/26
日志服务器和我的电脑IP段：10.2.100.0/19
（mfs好处真的多，不仅有web端管理界面，而且后期如果要加硬盘就只需要改改配置文件，维护太方便了）
PS：mfs可以当成raid0来用，即软RAID。6块1TB的硬盘通过mfs就变成了1块5TiB的大硬盘了！

## 1 下载和查看官方文档

我是按照它一步步来的，本文可以配合官方文档来食用...
提供个官网，以备不时之需：


如果你是Ubuntu、MACOS系统，我的操作会和你有点不一样，这点可以在官方文档里找到解决方案
我会在本文最后列出一些参考网站，希望能对你有帮助。本文只是简单搭建，具体配置和优化还请自行百度。

## 2 系统要求

mfs要求使用FUSE，需要Linux API 7.8或以上，即内核版本号要在2.6.0以上。然后因为有些的Linux可能有bug，所以版本号最好在2.6.24以上。
centos7或以上就不需要管了肯定没问题，我刚好是6，所以用`uname -a`来查看内核版本号
我是2.6.32，幸好我支持...
其他要求就在文档里了，什么内存不小于多少多少，如何计算你需要多少空间啥的，没啥用我就不列出来了
【请在root下操作，请暂时关闭防火墙（centos6为iptables）或添加防火墙放行规则】

## 3 一些概念

我真不是复制黏贴党，简单说说Mfs安装的东西是什么以及有什么用。
Master Server（元数据服务器）：就是一个主服务器，用于控制存储服务器们
MetaLogger（元数据日志服务器）：备份Master服务器的变化日志文件，当master服务器损坏，这服务器可以恢复文件。【说白了如果不需要这个服务器可以不搭建】
Chunk Server（数据存储服务器）：这就是真正存数据的服务器了，里面挂载了大量的硬盘用来存数据，上面说的Master就是用来控制这个服务器的。
Client（客户端）： 可以像挂载NFS一样（但其本身不是NFS），挂载MFS文件系统。
一台服务器可以同时搭建Master Server+ Metalogger + Chunk Server + Client【究极四合一】

【根据上面的用途，可以得出结论：一般是搭建mfs的话是使用 一个Master和很多个这种Chunk Server，如果有需要的话还有个MetaLogger。客户端装个Client就完事了】

## 4 开搞，安装主服务Master Server

开局先来5条命令
`yum install rpm-build gcc gcc-c++ fuse-devel zlib-devel`
`useradd -s /sbin/nologin mfs`
`curl " http://ppa.moosefs.com/RPM -GPG -KEY - MooseFS " > /etc/pki/rpm -gpg/RPM -GPG -KEY -MooseFS`
\#centos7的话要将下面的el6改成el7
 `curl " http://ppa.moosefs.com/MooseFS-3-el6.repo " > /etc/yum.repos.d/MooseFS.repo`
更新一下yum
`yum update`
然后
`yum install moosefs-master moosefs-cgi moosefs-cgiserv moosefs-cli`
安装成功之后进入目录
`cd / etc/ mfs`
目录里有 cfg和 cfg.sample文件，通过diff我发现这两种文件是一样的...
我建议你先执行以下语句：
`cp mfsmaster.cfg mfsmaster.cfg.bak`
`cp mfsexports.cfg mfsexports.cfg.bak`
然后再是：
`cp mfsmaster.cfg.sample mfsmaster.cfg`
`cp mfsexports.cfg.sample mfsexports.cfg`
按照官方的说法，.cfg.sample文件是简单的例子，而.cfg是正在使用的配置文件，那既然这样我们就创建个.bak文件备份一下吧。
编辑mfsexports.cfg文件。这个文件是管理权限的，详细的文件意思可以百度或者看文档内的注释。
反正直接加上下面这条就好了
`你客户端的网络号/你客户端的掩码 / rw,alldirs,maproot =0`
（注意网络号不等于IP）
因为我电脑（win10）和日志服务器（CentOs）都在10.2.100.0/19网络中，所以我加的是
`10.2.100.0/19        /    rw,alldirs,maproot=0,password=mfs`
即允许10.2.100.0/19中所有主机访问 MFS根目录 可读写、允许挂载任何指定的子目录、映射为root、密码为mfs
使用`mfsmaster -a start`来启动，网友建议启动时都加上`-a`，具体区别请自行百度。
其中，`mfsmaster stop`是关闭，`mfsmaster restart`是重启，这些基本知识就不说了。
如果没有报错，那么安装就成功了，记得将主服务器设为开机启动
`chkconfig --add moosefs-master`
`chkconfig moosefs-master on`    

## 5 安装MFS元数据日志服务器

我没装，如果你要装可以看看本文最后面的参考资料。这个安装也简单。
【元数据日志守护进程是在安装master server 时一同安装的，最小的要求并不比master  本身大，可以被运行在任何机器上（例如任一台chunkserver），但是最好是放置在MooseFS master 的备份机上，备份master  服务器的变化日志文件，文件类型为changelog_ml.*.mfs。因为主要的master server  一旦失效，可能就会将这台metalogger 机器取代而作为master server。】

## 6 安装web监视服务

按照官方文档的说法，只要是个Linux机器都能装这个监视服务，只要装了以后输入Master的ip就可以了。
但是官方强烈建议给装了Master Server的机器都装上这个web监视服务
`yum install moosefs-cgi moosefs-cgiserv moosefs-cli`
修改hosts文件 `vi /etc/hosts`，添加一条规则： `这台服务器的IP mfsmaster`
然后就可以启动服务了
`service moosefs-cgiserv start`
web管理的URL是 `localhost:9425/mfs.cgi` ，其他电脑可以输入URL：`服务器IP:9425/mfs.cgi`进行访问
访问不了？看看防火墙设置！

## 7 安装存储服务器Chunk servers

想要安装数据服务器(chunkservers)，这些机器的磁盘上不仅要有适当的剩余空间，还要求操作系统遵循POSIX 标准（如：Linux, FreeBSD, Mac OS X and OpenSolaris）。
如果你的Chunk服务器和Master服务器不是同一台，记得安装mfs的依赖
`yum install rpm-build gcc gcc-c++ fuse-devel zlib-devel –y`
 `useradd -s /sbin/nologin mfs`
然后安装`yum install moosefs-chunkserver`
备份一下配置文件。
`cd /etc/mfs`
`cp mfschunkserver.cfg mfschunkserver.cfg.bak`
`cp mfshdd.cfg mfshdd.cfg.bak`
`cp mfschunkserver.cfg.sample mfschunkserver.cfg`
`cp mfshdd.cfg.sample mfshdd.cfg`
这个时候你在文件mfshdd.cfg中指定要使用哪些硬盘，然后启动服务就搞定了
比如假设我要使用sde和sdc两块硬盘，那么久
`chown -R mfs:mfs /mnt/sde`
`chown -R mfs:mfs /mnt/sdc`
然后在mfshdd.cfg文件中添加2行
`/mnt/sde`
`/mnt/sdc`
启动服务
`service moosefs-chunkserver start`
加入开机启动项
`chkconfig --add moosefs-chunkserver`
`chkconfig moosefs-chunkserver on`
安装完成
但是我的服务器插了6块1T的硬盘还没挂载...我6块硬盘分别是 sdb sdc sdd sde sdf sdg
于是我要先将它们格式化，然后设置成开机自动挂载，然后挂载。
`mkfs -t ext3 /dev/sdb`
`mkfs -t ext3 /dev/sdc`
`mkfs -t ext3 /dev/sdd`
`mkfs -t ext3 /dev/sde`
`mkfs -t ext3 /dev/sdf`
`mkfs -t ext3 /dev/sdg`
所以我要用上面的格式化指令先格式化6块盘
然后在mnt目录下创建6个目录
`mkdir -p /mnt/sdb`
`mkdir -p /mnt/sdc`
`mkdir -p /mnt/sdd`
`mkdir -p /mnt/sde`
`mkdir -p /mnt/sdf`
`mkdir -p /mnt/sdg`
然后挂载我亲爱的STAT盘
`mount /dev/sdb /mnt/sdb/`
`mount /dev/sdc /mnt/sdc/`
`mount /dev/sdd /mnt/sdd/`
`mount /dev/sde /mnt/sde/`
`mount /dev/sdf /mnt/sdf/`
`mount /dev/sdg /mnt/sdg/`
更改权限
`chown -R mfs:mfs /mnt/sdb
chown -R mfs:mfs /mnt/sdc
chown -R mfs:mfs /mnt/sdd
chown -R mfs:mfs /mnt/sde
chown -R mfs:mfs /mnt/sdf
chown -R mfs:mfs /mnt/sdg`
然后还得开机自动挂载，要不然重启一下chunk服务器就找不到磁盘咯
`ll /dev/disk/by-uuid/`查找所有硬盘的UUID
然后`vim /etc/fstab`，在最后面加上6行（我UUID就删了哈，不给你们看）
`UUID= /mnt/sdb ext3 defaults 0 0
UUID= /mnt/sdc ext3 defaults 0 0
UUID= /mnt/sdd ext3 defaults 0 0
UUID= /mnt/sde ext3 defaults 0 0
UUID= /mnt/sdf ext3 defaults 0 0
UUID= /mnt/sdg ext3 defaults 0 0`
最后在/etc/mfs/mfshdd.cfg下增加6行
`/mnt/sdb
/mnt/sdc
/mnt/sdd
/mnt/sde
/mnt/sdf
/mnt/sdg`
启动chunk服务，增加chunk服务开机自动启动，完事。

## 8 验证安装结果

所有服务都搭载在210.38.69.12上，没有问题

![image-20220505060526805](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/05/060526-91.png)

至此，mfs搭建完成

## 参考资料

MFS官网：http://www.moosefs.com
MFS分布式文件系统：https://blog.51cto.com/13555423/2082796
MFS分布式文件系统：https://cloud.tencent.com/developer/article/1520226
Linux的企業-分布式文件系統mfs（moosefs）搭建與配置：https://www.itread01.com/content/1508786402.html
