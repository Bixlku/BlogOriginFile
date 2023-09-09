---
title: Linux服务器踩坑记
date: 2022-02-06 00:12:23
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

## 定时执行

定时执行可以使用`crontab -e`，具体使用方法参考[菜鸟教程](https://www.runoob.com/linux/linux-comm-crontab.html)

常见的：

```bash
*/5 * * * * /home/yyh/test.sh  #每五分钟执行一次test.sh
* */2 * * * /sbin/service httpd restart #每两个小时重启一次apache
```

