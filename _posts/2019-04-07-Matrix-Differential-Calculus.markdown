---
layout: post
title:  "矩阵微分 矩阵求导"
date:   2019-04-07 13:06:00
categories: math
tags: Matrix_calculate
mathjax: true
---

* content
{:toc}
### 写在前面

强烈建议优先看http://10.3.200.202/cache/10/03/web.stanford.edu/8e61bd0b459595c3aec3c97fbdee9c74/gradient-notes.pdf

看完后可以不看本博客，尽管我试图简化和条理化，博客内容仍然显得繁杂。

博客内容概况：矩阵求导后，存在不同的布局方式，导致了不同的求导运算。而神经网络计算中，要求矩阵求导后的形状和分子(也被称作自变量或变元)一致，以便于梯度下降操作。本博客试图厘清不同布局方式下矩阵求导的形状，以及对应的求导规则。



### 正文
公式markdown还不会用，直接传图了，源文件在图下面

#### 预定义符号
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-04-07-1.jpg)

#### 正文内容
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-04-07-2.jpg)
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-04-07-3.jpg)
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-04-07-4.jpg)
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-04-07-5.jpg)
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2019-04-07-6.jpg)

### 源文件：
需要用office365打开
https://github.com/GinSoda/Storage_Public/blob/master/Matrix%20differential.docx?raw=true

### 参考目录：
http://10.3.200.202/cache/10/03/web.stanford.edu/8e61bd0b459595c3aec3c97fbdee9c74/gradient-notes.pdf

https://zhuanlan.zhihu.com/p/32368246

《矩阵分析与应用 第二版》张贤达

《Matrix Differential Calculus with Applications in Statistics and Econometrics 3rd Edition》

https://en.wikipedia.org/wiki/Matrix_calculus
