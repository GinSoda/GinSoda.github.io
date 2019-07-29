---
layout: post
title:  "ESXi系统虚拟机直连NVIDIA显卡"
date:   2018-08-17 15:14:54
categories: Virtual_machine Server GPU
tags:  Ubuntu Virtual_machine GPU ESXi vSphere Server NVIDIA
---

* content
{:toc}

最近在服务器虚拟机上创建深度学习环境，遇到了一些问题，第一个就是NVIDIA不允许游戏显卡比如1060、1080在虚拟机中工作，NVIDIA官网说只有GRid或者Tesla显卡才支持直通，所以策略应该是，让显卡驱动认为你是物理机就OK了。下面我们解决一下这个问题。



## 步骤

#### 1、添加显卡到虚拟机

虚拟机编辑设置——>添加其他设备——>PCI设备

#### 2、不对虚拟机公开 NX/XD 标记(这个步骤可能无所谓)

ESXi6.0：虚拟机编辑设置——>不对虚拟机公开 NX/XD 标记

EXSi6.5：虚拟机编辑设置——>CPU——>硬件虚拟化——>取消向客户机系统公开硬件辅助的虚拟化

#### 3、正常安装显卡驱动(网上有大量资料，不再赘述)

#### 4、修改虚拟机的.vmx 配置文件

问题：安装驱动之后nvidia-smi出错，nvidia-smi reports Unable to determine the device handle for GPU ，可能是vmware直通的问题，修改其虚拟化的参数；

此处需要修改一下exsi中的虚拟机vmx配置，找到.vmx文件，在其底部添加`hypervisor.cpuid.v0 = "FALSE"`

注：此操作必须在ESXi6.5及以上环境进行，否则虚拟机启动报错：TSC_DEADLINE disabled due to Errata; please update microcode to version: 0x3a

#### 5、配置深度学习环境(网上有大量资料，不再赘述)

#### 6、直连同一显卡的虚拟机不能同时工作

## 参考目录

#### 添加显卡到虚拟机

https://docs.vmware.com/cn/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-C597DC2A-FE28-4243-8F40-9F8061C7A663.html

https://blog.csdn.net/fyl_1024/article/details/77980779

https://devtalk.nvidia.com/default/topic/982322/linux/nvidia-smi-reports-unable-to-determine-the-device-handle-for-gpu/

#### 显卡直连虚拟机

http://www.newsmth.net/nForum/#!article/DigiHome/652013?p=1

http://www.newsmth.net/nForum/#!article/DigiHome/686247


#### 显卡驱动安装

https://blog.csdn.net/red_stone1/article/details/78727096

http://brunoliu.com/2018/05/21/ubuntu-16-04-cuda-pytorch/

#### 深度学习环境搭建

https://segmentfault.com/a/1190000011703955

https://www.jiqizhixin.com/articles/2017-10-02-4

https://blog.csdn.net/lixiaoguang20/article/details/53669253