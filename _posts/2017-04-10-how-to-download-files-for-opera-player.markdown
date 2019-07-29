---
layout: post
title:  "如何给老年唱戏机下载有声小说"
date:   2017-04-10 22:14:54
categories: life
tags: MP3_player
---

* content
{:toc}

最近给家中长辈的唱戏机下有声小说，收集了各种资料，也遇见了一些问题，故在此总结一下。

注：唱戏机本质上就是MP3播放器



### 所需工具

1、IDM，主要用来下载有声小说，因为一些网站不提供下载功能，可以使用IDM进行播放触发式下载

软件下载地址：http://www.internetdownloadmanager.com/download.html

2、批量修改文件名3.7（batchReName3.7），主要用来批量修改有声小说文件名（其中的“序号”功能比较好用）

软件下载地址：http://www.binfensoft.cn/archives/300

3、闪存式MP3伴侣 v2.03，唱戏机处理TF卡中的文件时不是按名称排序，而一般是按写入TF卡时间排序，要想正确的排序文件就需要这个软件

软件下载地址：http://www.pc6.com/softview/SoftView_9455.html

4、酷狗的格式转换工具，用于m4a格式转mp3，因为姥姥的唱戏机只能处理mp3格式

软件下载地址：http://www.kugou.com/

### 有声小说平台

1、中央人民广播电台http://www.radio.cn/及其APP：中国广播

2、喜马拉雅FM

3、网易云音乐主播电台

4、有声听书、56听书、听世界

5、博听网

### 下载流程

下面以网易云音乐平台为例演示一下：

首先打开网易云音乐，找到“主播电台”，然后点击“有声小说”栏目，点击某一节目，比如“人民的名义”，点一下“节目列表”里的节目，然后ctrl+A选中全部节目，单击右键，点击“下载”
![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2017-04-10-1.png)
![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2017-04-10-2.png)
![](https://raw.githubusercontent.com/MaHaLo-G/Storage_Public/master/2017-04-10-3.png)
然后，打开电台文件所在位置，用批量修改文件名3.7修改文件名称，一般是数字在前，小说名在后。

最后，将文件拷贝到U盘，打开闪存式MP3伴侣 v2.03，单击右键，点击按文件名排序，再点击保存


### 展望

其实，感觉可以写一个爬虫工具在一些有声小说网站下载声音文件，然后写一些脚本，大大简化操作。但是老夫不会～

## 一些注意点

*  其实如果是从喜马拉雅fm下载声音文件，要自己用IDM一个一个下载并且改文件名（也可以写个小软件，把名字的散列规律找出来或者直接把名字从网页上传给IDM）
*  而且喜马拉雅fm和中广radio.cn的声音文件都是m4a格式，需要转换为mp3格式
*  注意检查有声小说集数是否完整，比如有声听书网上的文件可能没收录完整，需要自己核实。
*  注意使用“闪存式MP3伴侣”进行排序，否则可能在唱戏机上播放时，文件顺序会有些许错乱

---

   第一篇日志！
