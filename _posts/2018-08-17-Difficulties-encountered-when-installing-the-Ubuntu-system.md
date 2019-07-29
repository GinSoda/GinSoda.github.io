---
layout: post
title:  "安装虚拟机Ubuntu16.04系统一些坑"
date:   2018-08-17 14:14:54
categories: Virtual_machine
tags:  Ubuntu Virtual_machine
---

* content
{:toc}

最近在实验室服务器上安装虚拟机Ubuntu系统，遇到了一些坑，总结一下解决办法



## 问题

### 1、安装Ubuntu16.04时在选择时区步骤处，自动黑屏退出到桌面

原因：网络状况不佳，导致Ubuntu定位出错

解决方案：断网安装

### 2、Ubuntu16.04控制台中文乱码

#### 方法一：系统改为英文

修改Ubuntu的配置文件/etc/default/locale

将原来的配置内容修改为

LANG=”en_US.UTF-8″

LANGUAGE=”en_US:en”

再在终端下运行：

`$ locale-gen -en_US:en`

注销或重启后，Ubuntu Server真正服务器实体终端就恢复成了英文的语言环境。

#### 方法二：使用第三方shell比如zsh、putty、secuteCRT

#### 方法三：安装zhcon软件包

`$ sudo apt-get install zhcon`

即可将zhcon软件包安装上，它其实就相当于一个Ubuntu的UC-DOS程序，是一个汉字外挂。既然是外挂就必然要占用一定的系统资源，根据实际需求可选用该方法。

### 3、为Ubuntu16.04安装VMtools

#### 步骤一：找到菜单：虚拟机—>安装VMware Tools，点击安装VMware Tools，之后后自动挂载。

#### 步骤二：VMTools所在目录为：media/user/VMware Tools

#### 步骤三：ctrl+shift+F1打开控制台，转到VMTools所在目录

```
$ cd home
$ cd ../..
$ cd media/user/VMware Tools  #转到VMTools所在目录
$ mkdir /vmtools  #创建文件夹vmtools
$ tar xvf VMwareTools-10.0.0-2977863.tar.gz -C /vmtools  #解压到vmtools目录
$ cd /vmtools/  #转到此目录
$ cd vmware-tools-distrib  #转到此目录
$ ./vmware-install.pl  #执行perl文件
$ reboot  #重启虚拟机
```

### 4、鼠标错位

客户端显示器和虚拟机分辨率不一致，调到一致就ok了

## 参考目录

https://blog.csdn.net/guozhiyingguo/article/details/52852337

https://blog.csdn.net/ruoshuiss/article/details/78740213

