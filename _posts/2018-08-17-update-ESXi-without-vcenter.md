---
layout: post
title:  "ESXi6.0升级ESXi6.5 不用 VCenter和UpdateManger"
date:   2018-08-17 16:14:54
categories: Server
tags:  Server ESXi vSphere
---

* content
{:toc}

由于虚拟机直连NVIDIA游戏显卡在ESXi6.0下不行，需要ESXi6.5系统，所以进行升级。但网上很多教程是通过VCenter管理系统对ESXi系统进行升级，我们直接用U盘启动盘对ESXi系统升级。



## 步骤

### 一、U盘启动盘制作

1.没有vcenter时，无法通过vcenter管理平台在线升级，一般是直接下载ESXi6.5 iso做成U盘启动盘离线升级

2.下载地址为https://my.vmware.com/cn/group/vmware/evalcenter?p=free-esxi6 (需要先注册VMvare账户)

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-1.jpg)

3.并从www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/下载名为Universal USB Installer的实用程序，这是一个免费软件。

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-2.jpg)

 
4.选择Try Unlisted Linux ISO，设置iso路径后，一直enter就行。

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-3.jpg)

5.U盘启动盘制作完成。

### 二、ESXi6.5升级


1.将U盘插到服务器上，重启服务器时，按F11进入BIOS系统，选择从U盘启动
 

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-4.jpg)

2.欢迎界面，选择 Continue 下"Entert"键

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-5.jpg)

3.接受使用许可并安装。在下图，选择" accept and continue",按下"F11“

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-6.jpg)

4.安装位置，选择"continue",按下”Enter”键,如下图

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-7.jpg)

5.选择键盘布局，选择 US Default，按”Enter” 键

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-8.jpg)

6.输入最小长度为7为数的root 用户密码，选择“continue”，按“Enter”键，如下图

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-9.jpg)

7.确定信息安装，选择Install，按下“F11”键，如下图

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-10.jpg)

8.安装过程中，需要花较长时间，如下图所示

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-11.jpg)

9.安装完成，提示重新启动，按下“Enter”键，弹出提示框，不理，如下图

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-12.jpg)

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-13.jpg)

10.重启系统后，将会出现如下界面10-1，屏幕下方显示IP地址进行虚拟机管理，按F2输入用户密码, 自动跳转到VMwre配置设置界面，如图10-2所示

![10-1](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-14.jpg "10-1")

![10-2](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-15.jpg "10-2")

11.Enter键进入Troubleshooting Options

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-16.jpg)

12.打开 Enabled ESXi Shell 和Enabled SSH 选项，配置完成。如下图

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-17.jpg)

![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2018-08-17-18.jpg)

13.按ESC退出到主界面，至此升级完毕。vSphere client无法连接ESXi6.5，直接在客户机浏览器地址栏输入服务器ip即可访问ESXi6.5服务器系统。

## 参考目录

https://www.vladan.fr/how-to-upgrade-esxi-6-0-to-6-5-via-iso/

https://www.vladan.fr/how-to-create-a-usb-media-with-esxi-6-5-installation/

https://ithelp.ithome.com.tw/articles/10184288

https://forum.huawei.com/enterprise/zh/thread-383705.html