---
layout: post
title:  "补码与模运算"
date:   2021-03-23 21:23:54
categories: Computer_Science
tags:  two's_complement
---

* content
{:toc}

### 有限容量的计数系统、补数、模运算和同余数
#### 补数的定义
模（Modulo）：系统容量（比如钟表的模就是 12小时）

补数：a+b = 模，则**a和b对模Mod来说互为补数。补数的补数是本身**
#### 补数、模运算和同余数
b = -a + Mod，b和-a相差一个模，a%Mod 和 -b%Mod 取模运算结果相等，取余运算也相等。取模结果不一定等于取余结果。**b和-a对模Mod来说是同余数**

**a的补数和a的相反数是同余数：Mod-a和-a**

**a、a的相反数的补数、a的补数的相反数三者同余：a、a+Mod、a-Mod**

x和y相差为n倍系统容量值时，x与y模运算结果相同。比如在模=12时，{…, -7, 5, 17, 29, …}都是同余数。
#### 有限容量计数系统
计算机系统单个数字存储和钟表的容量都是有限的。

对同一计数系统中的数量可以定义运算如加减，但运算结果超出预设位数时，就要发生溢出，这个溢出其实就是模，是时钟的一整圈（因此丢掉它没有影响），如果进位没有被另一个计数系统接受，结果看似“失真”，本质上是进入了“第二次循环”。
![有限容量系统](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2021-03-23-1.jpg)
以时钟系统为例，8+7=15=13(十二进制)。进位丢失，表盘上只显示3。而8+(-5)也可以表示为3，这就说明在有限容量的计数系统中，+7和-5是完全相同的，而5和-7正是关于模12的同余数。

**所以不考虑进位时，在有限容量的计数系统中，对同余数（比如-a和b）进行加法，结果是一样的。**所以通过引入补数，把减正数（x-5）==> 加负数（x+(-5)）==> 加负数的补数（x+7）

#### 取模运算的意义
取模运算是一种映射，将 数字表示 映射到 系统数字的表示范围。(如下图红线所示)
![8容量系统](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2021-03-23-2.jpg)

### 原码、反码、补码
原码就是机器数，是加了一位符号位的二进制数，正数符号位为0，负数符号位为1

反码，英语里又叫ones' complement（对1求补），这里的1，本质上是一个有限位计数系统里所能表示出的最大值。求反又被称为对一求补，用最大数减去一个数就能得到它的反码，很容易看出**在二进制里11111111减去任何数结果都是把这个数按位取反**，0变1，1变零，所以才称之为反码。**反码只是一个过渡概念，用来连接求补数操作和位运算操作。**

补码，英语里又叫two's complement（对2求补），这个2指的是计数系统的容量（模），就是计数系统所能表示的状态数（容量等于最大数加一，因为多容纳了一个表示即0）。对7位二进制数来说容量就是10000000，这个模是不可能取到的，因为位数多一位。10000000=1111111+1，所以（10000000－1010010）稍加改变就成了（1111111－1010010）+1，所以补码又可以表述为先求反再加1。易知**补码的补码是本身。**

### 求补码
求补码不同于求补数，对非负整数来说，求补码就是原码本身；负数的补码是在其原码的基础上, **符号位不变**, 求数值位的补数（不是求负数的补数，是求负数绝对值的补数，**所以不考虑符号位时负数的补码(所代表的数字)和负数是同余数，模是2^31**）

x-y=x+(Mod-y)，减y等价于加y的补码，计算机只要部署加法电路和补码电路，就可以完成所有整数的加法。

由于计算机符号位的存在，**c++中求补数时，int的模是2^31，而非2^32。**
### 符号位
符号位不参与反码、补码运算

符号位参与加减法运算

符号位参与 与、或、非和异或 运算

符号位的左移右移：c++中负数不能左移，负数右移高位填充1；-1>>1还是-1，1>>1能得到0

在无符号位容量=256的系统中，-1和255互为同余数，所以两者二进制表示相同，都为0b1111 1111。实际上容量=256的系统中0b1111 1111可以表示{…, -257, -1, 255, 511, …}中任何一个数。
#### 符号位参与加法的变化分析
跨-128（溢出）导致的符号变化：同符号数相加只会跨过-128，不会跨过0

正数加正数若溢出，符号会0变1，变成负数；负数加负数若溢出，符号位会1变0，变成正数。

0变1的，说明大得越界了，需要再求个补，用取值范围内的负数表示。1变0的，说明小得越界了，但由于正数的补数就是本身，就不必再求补了
### ~x+1

![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2021-03-23-3.jpg)

### 补充：取模和取余
#### 主要区别
取模运算（“Modulus Operation”）和取余运算（“Remainder Operation ”）两个概念有重叠的部分但又不完全一致。主要的区别在于对负整数进行除法运算时操作不同。取模主要是用于计算机术语中。取余则更多是数学概念。

取余，遵循尽可能让商向0靠近的原则

取模，遵循尽可能让商向负无穷靠近的原则

符号相同时，两者不会冲突。
#### 不同编程语言下的实现
各个环境下%运算符的含义不同，比如c/c++，java 为取余，而python则为取模。
![](https://raw.githubusercontent.com/GinSoda/Storage_Public/master/2021-03-23-4.jpg)

### 参考来源
www.cnblogs.com/zhangziqiu/archive/2011/03/30/computercode.html

www.zhihu.com/question/23172611/answer/119406298

www.baike.baidu.com/item/取模运算

www.zhihu.com/question/30526656/answer/150919770