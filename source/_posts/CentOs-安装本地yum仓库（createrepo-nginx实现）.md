---
title: CentOs 安装本地yum仓库（createrepo+nginx实现）
date: 2020-10-30 16:07:12
tags: [CentOs, createrepo]
author: VsKendo
categories: 运维
summary: LaTeX是一种基于TeX的文档排版系统，成为现在最流行的科技写作——尤其是数学写作的工具之一.
description: LaTeX是一种基于TeX的文档排版系统，成为现在最流行的科技写作——尤其是数学写作的工具之一.
---

## 0. 目的

想要建立私有Yum仓库，方便运维。
我感觉搭建好以后，甚至可以当作自己的对象存储oss来用

## 1. 开始

安装的系统为CentOs6。

```
Linux gdei 2.6.32-754.el6.x86_64 #1 SMP Tue Jun 19 21:26:04 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

服务器地址为：192.168.219.130（VM虚拟机）
先安装nginx，要不然没有网页端服务。不需要网页端的可以跳过。

## 2. 暂时关闭防火墙

关闭iptables :

```
service iptables stop
```

关闭selinux:
查看SELinux状态：

```
/usr/sbin/sestatus -v      ##如果SELinux status参数为enabled即为开启状态
getenforce                 ##也可以用这个命令检查
```

关闭SELinux：
1、临时关闭（不用重启机器）：

```
setenforce 0                  ##设置SELinux 成为permissive模式
                              ##setenforce 1 设置SELinux 成为enforcing模式
```

2、修改配置文件需要重启机器：
修改/etc/selinux/config 文件
将SELINUX=enforcing改为SELINUX=disabled
重启机器后关闭selinux

## 3. 安装createrepo

准备软件仓库目录：

```
mkdir /home/data -p
```

使用yum的只下载不安装的方式进行下载软件(下载啥都行，此处以下载gcc为例，你也可以自己打包rmp然后放到仓库)

```
yum install --downloadonly --downloaddir=/home/data gcc
```

下载creatrepo：

```
yum -y install createrepo
```

开始运行服务：

```
createrepo /yum/downloade/
```

现在，自定义yum仓库已经搭建完成，目前只有gcc这一个包可供下载

## 4. 更新

更新源数据：如果有新的软件包放入到软件仓库，这里就需要更新元数据。
也就是说每次将数据放入这个文件夹下想共享出去，放到这下面以后就要使用下面的命令更新一次。

```
createrepo --update /home/data
```

至此，搭建完成。

## 5. 使用Nginx制造网页端查看页面

要安装Nginx之前要安装epel，要不然装不了nginx

```
yum install epel-release
```

然后安装nginx

```
yum install nginx
```

开始配置nginx，配置完毕后网页端就能访问了

```
cd /etc/nginx/conf.d/
```

备份配置文件

```
cp default.conf default.conf.bak
```

开始修改配置文件

```
vi default.conf 
```

修改成如下的样子

```
server {
    listen       80;
    server_name  192.168.219.130;
    root         /home/data;

    location / {
    autoindex on;
    autoindex_exact_size off;
    autoindex_localtime on;
    autoindex_format html;
    charset utf-8,gbk;
    root         /home/data;
    }

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }

}
```

## 6. 成功

访问192.168.219.130（服务器IP），成功！效果如下：

![image-20220505060035049](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/05/060035-e2.png)

点击文件，可以下载:

![image-20220505060047071](https://vskendo-1255590242.cos.ap-guangzhou.myqcloud.com/2022/05/05/060047-03.png)

## 7. 使用

需要使用yum的客户端，需编辑配置文件，命令操作如下所示：

```bash
cd /etc/yum.repos.d/
cp CentOS-Base.repo CentOS-Base.repo.bak
vim CentOS-Base.repo
[base]                                     //中括号里内容要求唯一，但不要出现特殊字符
name=A nice description           //此为描述信息，可以看情况填写
baseurl=http://192.168.219.130/                    //此项为yum软件仓库位置
enabled=1                                   //此项为是否开启，1为开启, 0为不开启
gpgcheck=1                                  //此项为是否检查签名，1为检测, 0为不检测
cost=1000    # 默认yum仓库cost为1000, cost越小越优先
gpgkey=0  //签名认证信息的路径
//有些baseurl设置的稍微复杂，比如：
//
$releasever/os/$basearch/
//其中
//    $releasever    # 只替换主版本号
//    $basearch # 系统基本架构
//具体配置请自己根据yum的配置知识来陪
```

运行以下命令生成缓存 ：

```bash
yum clean all
yum makecache
```

（因为有备份文件，随时可以换回来，所以不怕翻车）
附上阿里云的yum源配置文件下载（如果你是Centos7就将这个6换成7）

```bash
wget http://mirrors.aliyun.com/repo/Centos-6.repo
//用wget下载后，放到/etc/yum.repos.d/文件夹下，并改名为CentOS-Base.repo
//再运行上面2个命令重新生成缓存即可。
```

## 8.参考

搭建yum私有仓库：https://www.jianshu.com/p/c7c2dea549b1?open_source=weibo_search
搭建私有YUM仓库与内网镜像站：http://www.360doc.com/content/19/0819/08/36367108_855782032.shtml
创建私有yum仓库：https://www.cnblogs.com/zakzhu/p/11632971.html
Linux yum 命令：https://www.runoob.com/linux/linux-yum.html
搭建私有yum仓库：https://www.cnblogs.com/37yan/p/6879287.html
