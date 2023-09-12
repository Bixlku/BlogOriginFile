---
title: Linux服务器踩坑记
date: 2023-09-10 10:45:00
tags: 折腾
---

# Linux服务器踩坑记

## Ubuntu的安装

在安装过程中，Ubuntu Server多次安装错误，解决方法是换一个u盘。。。

## DDNS服务

DDNS服务选用的是园长给的教程，需要注意的是，DNS服务是有刷新时间的，有时候看上去没更新，实际上是DNS服务还在刷新中，等待即可。

学校的有线网是有IPv6的，而无线网是IPv4的。

首先的情况是，园长给的教程是[园长的DDNS教程](https://iamddch.com.cn/2020/09/04/CloudflareBasedDDNS/)，其中给出的sh脚本存在一个问题，我在使用的时候无法对内网的ipv4地址进行同步，其报错内容为需要一个`valid ip address`，分析认为该脚本无法将内网ip地址上传至cloudflare服务器。而将ipv6地址进行同步的工作则较为顺利。

这就带来一个问题，我在校园网中是用不了ipv6的，而ipv4地址因为动态DHCP服务会一直改变，到后面远在上海的园长可以ssh上我的服务器，而在北京的我却要找园长`ifconfig`一下才能拿到本机的内网ipv4地址。

于是乎我以试试看的心态上网找了另外一个DDNS脚本，即[DDNS脚本](https://github.com/wherelse/cloudflare-ddns-script/tree/master)，运行后效果良好，能够正确的将本地ipv4地址同步至cloudflare。

## 未解之谜

我的机器物理内存是8G，但是`free -g`命令之后得到的总空间只有4G，暂时还是不明白原因

![image-20230909194642717](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230909194642717.png)

### 未解之谜的解决

园长发现，缺失的ram应该是分配给了gpu用作核显显存

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230909224149495.png" alt="image-20230909224149495" style="zoom:80%;" />

## 定时执行

定时执行可以使用`crontab -e`，具体使用方法参考[菜鸟教程](https://www.runoob.com/linux/linux-comm-crontab.html)

常见的：

```bash
*/5 * * * * /home/yyh/test.sh  #每五分钟执行一次test.sh
* */2 * * * /sbin/service httpd restart #每两个小时重启一次apache
```

## 常用命令

`ifconfig`查看当前机器网络状态，按网卡分类

### iperf3是个好工具

`iperf3 -s`当前机器作为服务端

`iperf3 -c ipaddress` 当前机器作为客户端，对特定ip地址进行网速测试

## 杂七杂八

脚本文件必须要`chmod +x`将权限调整之后才能进行运行

登陆的端口已经改成35791了，在ssh后面需要加`-p 35791`

### 防火墙

园长已经为系统设置防火墙，故刚才的iperf3的速度测试打不出来速度

`ufw status` 查看开放的端口

`ufw allow xxx` 打开端口xxx

`ufw delete allow xxx` 关闭端口xxx

## 猫猫

想在linux养一只猫猫，养猫教程目前参考的这篇[养猫](https://blog.zzsqwq.cn/posts/how-to-use-clash-on-linux/)

### 解压问题

对`.gz`文件的解压，使用`gzip -d xxx.gz`，对`.tar`文件的解压，使用`tar -xf xxx.tar`，对`tar.gz`或`.tgz`文件的解压，使用`tar -xzf xxx.tgz/xxx.tar.gz`具体参见[菜鸟教程](https://www.runoob.com/w3cnote/linux-tar-gz.html)

### 挂载U盘问题

查看目前系统的挂载情况`fdisk -l`

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230910105646867.png" alt="运行结果" style="zoom:80%;" />

此处挂载的应该是分区，而不是设备id，即应当选择挂载/dev/sdb1而不是挂载/dev/sdb

挂载命令：`mount /dev/sdb1 /dev/usb` 需要注意的是，若dev目录下没有usb文件夹，则需要先手动`mkdir /dev/usb`，再进行挂载

取消挂载命令：`umount /dev/sdb1`

### docker问题

没啥好讲的，参考的这篇[docker教程](https://yeasy.gitbook.io/docker_practice/install/ubuntu)

## Seafile部署

养猫去了把这茬给忘了，下次再写

使用docker部署Seafile服务器，参考的是官方文档[用Docker部署Seafile](https://cloud.seafile.com/published/seafile-manual-cn/docker/%E7%94%A8Docker%E9%83%A8%E7%BD%B2Seafile.md)，因此学习了一些Docker的基本命令。目前已经在服务器上部署了seafile服务，登陆账户和注册账户已经可以跑了，但是暂时还没搭建数据库，存不了东西

### docker的相关命令

可参考[菜鸟教程](https://www.runoob.com/docker/docker-container-usage.html)

列出本地镜像 `docker images` 

列出xxx的本地镜像 `docker images xxx` 

启动已经停止运行的容器 `docker start ContainerID`

停止一个容器 `docker stop ContainerID`

删除一个容器 `docker rm -f ContainerID`

### MySQL

MySQL挺难的。。。还得认真学一学

