---
title: Linux服务器踩坑记
date: 2023-09-13 11:15:00
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

DDNS服务器脚本是有问题的，明天需要改一下脚本的内容，然后综合到se.yyhnet.top里面去

最后使用[ddns整合包](https://github.com/NewFuture/DDNS)解决了问题，然后写了一个bash放进crontab里面设置为定时任务。写了一个命令以获得内网ip地址`ifconfig enp1s0 | grep 'inet ' | awk '{print $2}'`解析一下：首先是调出了网卡enp1s0的所有信息，而后选择inet （记得inet后面有个空格）的这一行，awk即选择字段2，就可以得到inet后面的ip地址

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

`-p port`指定端口

## 杂七杂八

脚本文件必须要`chmod +x`将权限调整之后才能进行运行，`chmod 777`也行

登陆的端口已经改成35791了，在ssh后面需要加`-p 35791`

### 防火墙

园长已经为系统设置防火墙，故刚才的iperf3的速度测试打不出来速度

`ufw status` 查看开放的端口

`ufw allow xxx` 打开端口xxx

`ufw delete allow xxx` 关闭端口xxx

`ufw enable` 防火墙打开

`ufw disable` 防火墙关闭

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

使用docker部署Seafile服务器，参考的是官方文档[用Docker部署Seafile](https://cloud.seafile.com/published/seafile-manual-cn/docker/用Docker部署Seafile.md)，因此学习了一些Docker的基本命令。目前已经在服务器上部署了seafile服务，登陆账户和注册账户已经可以跑了，但是暂时还没搭建数据库，存不了东西。

发现不是数据库的问题，使web端的seafile地址没设置好，下图需要设置成自己的URL才能够存放文件![image-20230912173209371](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230912173209371.png)

现在的问题是只有http而没有https证书，等会需要问一下园长有没有搭nginx，考虑走nginx

### docker的相关命令

可参考[菜鸟教程](https://www.runoob.com/docker/docker-container-usage.html)

列出本地镜像 `docker images`

列出所有本地镜像的状态 `docker ps -a`

列出xxx的本地镜像 `docker images xxx`

启动已经停止运行的容器 `docker start ContainerID`

停止一个容器 `docker stop ContainerID`

删除一个容器 `docker rm -f ContainerID`

## Nginx

Nginx是一个反向代理服务，我在服务器中的使用是：外部主机域名访问24680端口时，会重定向到本机的80端口，并进行https加密

园长将Nginx安装于`/usr/local/nginx`位置，其中需要注意的是`conf`文件夹和`sbin`文件夹，`conf`文件夹中的`nginx.conf`是nginx的配置文件，`sbin`文件夹中的`nginx`是nginx程序的执行文件

### 普通反向代理

具体教程可以看[程序员自由之路](https://www.cnblogs.com/54chensongxia/p/12938929.html)，常用功能简单说明一下

nginx.conf文件分为三个部分：全局块、event块、http块

例子：

```shell
#全局块
#user  nobody;
worker_processes  1;

#event块
events {
    worker_connections  1024;
}

#http块
http {
    #http全局块
    keepalive_timeout  65;
    #server块
    server {
        #server全局块
        listen       8000;
        server_name  localhost;
        #location块
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    #这边可以有多个server块
    server {
      ...
    }
}

```

做反向代理需要考虑的是http块，http块由http全局块和server块构成，重要的是server块

一个最简单的无加密的反向代理server块如下。

```bash
server{
	listen listen_port;#监听端口
	server_name xxx.xxx.xxx;#域名，没有域名的话也可以不写
	client_max_body_size 10000m;#这里极为重要，因为nginx默认是设置最大文件传输大小为1m，如果不改变的话传输的时候就会卡死
	
	location /{
		proxy_pass http://127.0.0.1:proxied_port;#被反向代理的端口服务
	}
}
```

### HTTPS加密

https加密我这里使用的是certbot，参考的是[Cerbot教程](https://kuokuo.io/2019/08/05/get-lets-encrypt-cert/)。Certbot是Let's Encrypt的官方推荐证书申请工具

```bash
sudo apt update && sudo apt install certbot#安装Certbot
```

```bash
sudo certbot -h #看输出，以确定Certbot已经安装完毕
```

关闭80端口和443端口的程序

``` bash
lsof -i:port#查看端口号占用情况，然后kill或者用其他方法关闭
```

无静态目录的做法：

```bash
sudo certbot certonly --standalone -d example1.com -d example2.com
```

有静态目录的做法：

```bash
sudo certbot certonly --webroot -w /var/www/example -d example.com -d www.example.com
```

此时会要求输入邮箱，运行结束后需要记下证书的存放位置，例如

![image-20230913114416105](https://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/image-20230913114416105.png)

接下来进入nginx.conf文件进行配置

```bash
server {
        server_name example.com www.example.com;
        listen 443 ssl;#设置为ssl
        
        # ssl on; ssl on这个方法已经被废弃，现在只需要在listen后面加ssl即可
        ssl_certificate 路径/fullchain.pem;
        ssl_certificate_key 路径/privkey.pem;
		client_max_body_size 10000m;#这里极为重要，因为nginx默认是设置最大文件传输大小为1m，如果不改变的话传输的时候就会卡死
        location / {
           proxy_pass http://127.0.0.1:proxied_port;
        }
    }
```

退出保存，进入nginx的sbin文件夹，然后重启nginx

```bash
sudo ./nginx -t#查看测试代码，如果successful就可以重启
sudo ./nginx -s reload
```

### 命令

nginx命令也就那么几条

```bash
nginx -t #测试nginx.conf文件是否可用
nginx -s reload #重新载入配置文件并运行
nginx -s stop #快速停止nginx
nginx -s quit #停止nginx（推荐）
nginx -v #查看nginx版本
```

## 虚拟化

折腾了一下午，终于实现了Windows10在Ubuntu Server下的虚拟化运行，目前配置为双核四线程，CPU性能损失很小，GPU似乎暂时存在兼容性问题，待处理

### 安装过程

参考文章：[KVM-Atlas](https://cloud-atlas.readthedocs.io/zh_CN/latest/kvm/install/ubuntu_deploy_kvm.html)、[在 Ubuntu 的 KVM 中安装 Windows 系统](https://zhuanlan.zhihu.com/p/24764017)、[KVM-Qemu-Libvirt三者之间的关系](https://hsinin.github.io/2017/01/16/KVM-Qemu-Libvirt%E4%B8%89%E8%80%85%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB/)、[virsh常用命令](https://www.cnblogs.com/cyleon/p/9816989.html)

#### 准备工作

```bash
$ egrep -c '(vmx|svm)' /proc/cpuinfo #查看CPU是否支持虚拟化技术，输出0表示不支持
$ egrep -c ' lm ' /proc/cpuinfo #大于0则为64位处理器
$ uname -m #检查内核，若x86_64则为64位内核
```

#### 安装KVM

```bash
$ sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst #我也说为什么安装的时候有red hat的表示，原来此处的virt-install就是red hat提供的
```

#### 添加用户组

这一步不知道有什么用，反正加进去就完事了，如果用户权限这块出了问题就去找这个[Ubuntu部署KVM](https://cloud-atlas.readthedocs.io/zh_CN/latest/kvm/install/ubuntu_deploy_kvm.html)

```bash
sudo adduser `id -un` libvirt
```

#### 创建Windows虚拟机

大坑要来了

创建虚拟机配置文件的写法（此处我将virt-install命令写成了一个脚本文件，所以称之为配置文件，实际上与直接输入virt-install命令没有区别）

virtio驱动文件的下载地址[fedora](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/)，建议使用iso文件

qcow2文件和raw文件的区别可以参考[KVM-qcow2和raw格式对比](https://blog.csdn.net/sssssuuuuu666/article/details/106999198)

注意virtio.iso文件必须在--disk中以cdrom格式挂载，这里我折腾了好久才挂上

```bash
#!/bin/bash
virt-install \
  --network bridge=virbr0,model=virtio \ #网络连接方式
  --name win10 \ 
  --ram 2048 \ #内存大小，单位为MB
  --vcpus sockets=1,cores=2,threads=2\ #分配的cpu数量，此处的threads指的是每个核分配的线程数，类似这里cores=2,threads=2，到最后的vpcu就会等于2*2=4 
  --os-variant=win10 \
  --disk path=/var/lib/libvirt/images/vir-driver.iso,device=cdrom\ #驱动文件virtIO.iso的挂载。iso文件应当以cdrom方式挂载，且当主硬盘格式为qcow2时，必须在安装前挂载该virtIO-win驱动，否则会导致安装时系统无法识别主硬盘
  --disk path=/var/lib/libvirt/images/win10.qcow2,format=qcow2,bus=virtio,cache=none,size=40 \ #主硬盘挂载。任意选择一个地方放至该qcow2文件即可，文件系统建议选择qcow2，因为其空间要比raw好很多，size后面指的是主硬盘空间大小，单位为GB。
  --graphics spice \ #远程桌面连接协议，此处可选vnc和spice，建议选择spice，流畅度好很多，但是选择了spice之后似乎就不能指定端口了
  --cdrom /var/lib/libvirt/images/win10_ltsc.iso #系统镜像文件，以cdrom的形式挂载
```

若创建成功，则会有如下显示

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230914180946439.png" alt="image-20230914180946439" style="zoom:80%;" />

```bash
Starting install...
Creating domain...
Domain installation still in progress. Waiting for installation to complete.
```

#### 远程端口映射到本地

不知道为什么这些远程桌面软件都要求对远程端口进行本地映射，也就是要求其只能连接127.0.0.1:ports

需要注意，此处的远程端口映射到本地指的是在客户端进行操作，即输入shell的是客户端，绝对不是服务器！！！！

```bash
ssh -C -L local_port:127.0.0.1:remote_port username@server.com
```

如果使用的是spice协议，则其默认端口为5900，即此处的remote_port为5900

运行后会让输入对应用户的密码，然后ssh登录，往前面翻看是否有报错，没有的话不要关闭这个窗口，到后台最小化即可





坑：本地端口映射到远程、创建虚拟机配置文件的写法（virt-install命令的写法、virtIO驱动的下载、virtIO驱动cdrom的挂载、Graphic中spice和vnc的选择、cpu核心数和线程数中线程对应的是总线程数还是每个core的线程数、各种kvm文件系统的区别）、KVM虚拟机的创建、启动、停止、删除、运行时虚拟机配置文件的写法（在虚拟机创建后还可以调整虚拟机的配置、vcpu的限制）、远程桌面的使用（windows自带的远程桌面、tight VNC、virt-manager，spice和vnc协议的选择）

报错：

`error: unsupported configuration: CPU topology doesn't match maximum vcpu count`



需要理解的：QEMU、KVM、Libvir、virsh关系和区别

仍需学习：虚拟机克隆
