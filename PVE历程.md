---
title: PVE+显卡直通历程
date: 2023-10-05 15:37:21
tags: 学习
---

# 环境介绍

机器：零刻SER5 Pro 5700U

系统环境：PVE 8.0 + Windows 11 专业版

本文参考视频：[AMD核显直通显示输出](https://www.bilibili.com/video/BV1ZN411n74e)

# 安装PVE

[PVE官方下载链接](https://www.proxmox.com/en/downloads)

下载最新的ISO Installer

ISO映像写入建议使用Etcher，简单快捷

安装过程没什么要写的，一路按需配置即可 Hostname 随意选择，写pve.lan就可以

然后登录PVE界面

# 更换清华源

需要注意的是，PVE基于debian系统，因此清华镜像记得选debian

换源修改`/etc/apt/sources.list`，参考 [清华debian官方文档](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)，[清华Proxmox官方文档](https://mirrors.tuna.tsinghua.edu.cn/help/proxmox/)，[清华ceph官方文档](https://mirrors.tuna.tsinghua.edu.cn/help/ceph/)

修改后，执行`sudo apt update`和`sudo apt upgrade`

# 修改DNS

![image-20231005155608535](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005155608535.png)

此处我选择的是阿里的公共DNS，修改DNS服务器2和DNS服务器3即可

然后使用`ping www.baidu.com`确定主机可以连接到外网，需要注意的是，PVE这个系统的ping命令很慢，要多等一会才会有显示

# 编辑GRUB

```bash
nano /etc/default/grub
```

在grub文件中在`GRUB_CMDLINE_LINUX_DEFAULT`选项中的`quiet`后面加上`initcall_blacklist=sysfb_init`

![image-20231005160958121](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005160958121.png)


更新grub
```bash
update-grub 
```

修改blacklist文件

```bash
nano /etc/modprobe.d/pve-blacklist.conf
```

在其后添加

```bash
blacklist amdgpu
blacklist i915
blacklist snd_hda_intel
options vfio_iommu_type1 allow_unsafe_interrupts=1
```

更新initramfs

```bash
update-initramfs -u -k all
```

重启

```bash
reboot
```

更新pcie设备

```bash
update-pciids
```

显示显卡的ID

```bash
lspci -D -nn | grep VGA
```

显示声卡的ID

```bash
lspci -D -nn | grep Audio
```

记录下面的信息，到后面有用

![image-20231005161717463](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005161717463.png)

# 提取VBIOS和GOP Driver文件

使用AFUWIN软件，Windows PE进入主机中，提取出BIOS文件

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005164015286.png" alt="image-20231005164015286" style="zoom:80%;" />

可以用这个[AFUWIN软件](https://www.bilibili.com/read/cv17969975/)，来提取BIOS文件，然后得到的就是一个这样的文件`afuwin.rom`

将`afuwin.rom`文件放入UBU文件夹，运行UBU.bat

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005164738454.png" alt="image-20231005164738454" style="zoom:80%;" />

按任意键

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005164811701.png" alt="image-20231005164811701" style="zoom:80%;" />

输入2，回车

<img src="http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005164846160.png" alt="image-20231005164846160" style="zoom:80%;" />

输入S，回车

而后就可以找到同文件夹中有一个Extract文件夹，里面有两个文件夹

![image-20231005165015242](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20231005165015242.png)

其中5000系列的VBIOS都是直接给出的，在VBIOS文件夹中。GOP Driver在GOP文件夹中。

下载`edk2-BaseTools-win32-master`软件，用cmd进入软件目录下，将`AMDGopDriver.efi`文件复制到软件目录下，运行

```bash
EfiRom.exe -f 0x1002 -i 0xffff -e AMDGopDriver.efi #注意，此处的0xffff中的ffff是显卡对应的设备号，例如此处应当是是164c。前面的0x1002中的1002代表AMD
```

在同一文件夹下会出现`AMDGopDriver.rom`文件，将该 rom文件 和 VBIOS文件夹中的vbios文件 传送到服务器中，然后将该文件复制到`/usr/share/kvm`文件夹中，至此，准备工作已经完成，进入虚拟机安装程序。

# 虚拟机安装

下载Windows11安装镜像，下载Virtio-win驱动并挂载，这个操作比较简单，不赘述。

在系统页面下，选择TPM，版本选择2.0，机型q35，显卡选择无。

磁盘总线选择SCIS，磁盘大小不小于64GB

CPU类别选择host效率更高，注意此处的核心指的是线程数量

网络模型选择 VirtIO(半虚拟化) 效率更高。

调整启动顺序，并添加CD/DVD驱动器的驱动iso文件

上述具体操作可参考文中开头的那个视频，做的比较详细

# GPU直通

添加PCI设备，选择到显卡对应的设备号，勾选 主GPU 和 PCI-Express 

添加PCI设备，选择声卡设备号

添加USB键盘和鼠标，剩下来的东西后面再来添加。

## 调整conf文件

```bash
nano /etc/pve/qemu-server/虚拟机序号.conf
```

调整romfile文件的指向：

在`hostpci0`后面加上`romfile=vbios文件名`，在`hostpci1`后面加上`romfile=AMDGopDriver.rom`

运行`qm start 虚拟机序号`开机

此时即可见到屏幕亮起，完成系统的安装。

# 亮屏之后

记得安装Virtio的驱动程序，AMD核芯显卡的驱动需要自己进行安装，可能一开始不识别，但是只要屏幕亮了，就说明显卡直通成功了，GPU-Z会有显示的，打驱动即可。

