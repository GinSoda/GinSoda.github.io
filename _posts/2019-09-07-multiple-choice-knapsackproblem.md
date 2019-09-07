---
layout: post
title:  "多重背包单调队列解法图解"
date:   2019-09-07 17:23:54
categories: Algorithm
tags:  Knapsack_problem
---

* content
{:toc}

最近做算法题，多重背包的单调队列看了几遍[闫学灿同学](https://space.bilibili.com/7836741)的视频讲解没看懂，自己总结了一下。



### 多重背包的单调队列

物品数n=3，背包空间v=11

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-1.jpg)

#### 计算过程与转移公式

dp\[i\]\[j\]代表只考虑前i个物品, 背包容量为j时, 可获得的最大价值

转移公式：dp\[i\]\[j\] = max(dp\[i-1\]\[j\], dp\[i-1\]\[j-kc\] +kw), 1≤k≤s = max(dp\[i-1\]\[j-kc\] +kw) , 0≤k≤s

转移表如下：

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-2.jpg)

#### 只考虑求dp[2][1]时
									
绿色代表：背包容量v % 物品空间c = 0

橙色代表：背包容量v % 物品空间c = 1

绿色只从上一层绿色更新，橙色只从上一层橙色更新

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-3.jpg)

#### 只考虑绿色块的转移过程

绿色代表有效区域，灰色代表无效区域(由于只能向后转移，或者物品个数限制)。物品个数限制也可以理解为每行最多有s+1个元素。

黑色字体代表该行最大值

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-4.jpg)

上图这个过程可以通过单调队列实现

#### 另一种角度看转移过程

虚线代表物品个数限制导致的无效的转移

实线代表有效的转移

红色代表最大值那条转移

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-5.jpg)

#### 得到结果

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-6.jpg)

### 完全背包问题的另一种角度

完全背包问题转移图和多重背包一样。不过没有了物品个数限制，所以不是滑动窗口情况，不用单调队列。只保存一个最大值就可以。

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-09-07-7.jpg)

不一定全由dp\[1\]\[0\]转移过来，只是样例恰好如此

### 参考目录

https://www.zybuluo.com/RabbitHu/note/857837