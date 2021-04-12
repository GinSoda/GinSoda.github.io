---
layout: post
title:  "python中的负数补码表示"
date:   2021-04-12 12:23:54
categories: Computer_Science
tags:  two's_complement
---

* content
{:toc}

### python中int数据的表示和存储
python中对于int数据是以原码的形式展示，以补码的形式存储和位运算

不同于c++、java的int只有32容量，python是一个无限容量表示系统，实际存储中使用自动扩容机制。
### python中负数的原码与补码
c++中符号位在最高位，python中符号位在无穷最高位。

**python中负数补码高位会有无穷的1。**

**由于python并没有很好的机制在二进制中表示负数补码前面有无穷的1，所以python无法直接表示与获得负数的补码。**

```
print(bin(1)) # 0b1  
print(bin(-1)) # -0b1
```
### python负数补码转换为c++负数补码
```
a = -127  
# 方式一，将无穷位系统补码截断为32位系统补码。python中对int以补码的形式存储和位运算  
x = a & 0xffffffff  
# 方式二，将负数转换为对应的同余数（正数）。不考虑符号位时负数的补码(所代表的数字，肯定是个正数)和负数是同余数  
y = a + 0xffffffff + 1  
print(x==y) # Ture  
```
### c++负数补码转换为python负数补码
```
x = pow(2, 32) - 1 # c++ int32位系统中2^32-1代表 -1。  
'''方式一 
y = x ^ 0xffffffff代表对x在32位系统内取反，得到c++某个正数y的补码，等价于python正数y的补码。(y和x没啥关系，就是个媒介) 
~ (x ^ 0xffffffff) 代表对正数y在python无穷位系统内取反，得到python负数x的补码 
本质：python无法表示无穷的1，所以无法直接获得负数的补码。所以通过对无穷的0取反得到无穷的1，通过两次取反，正数作为媒介，间接得到对应的python负数补码 
注意32位系统表示范围0到2^32-1, 你输入x=pow(2,32)方案一肯定不对了  
'''  
a = ~ (x ^ 0xffffffff)  
# 方式二，将正数转换为对应的同余数（负数）。不考虑符号位时负数的补码(所代表的数字，肯定是个正数)和负数是同余数  
b = x - 0xffffffff - 1  
print(a) # -1  
print(b) # -1  
```
### 参考来源
ginsoda.github.io/computer_science/2021/03/23/two's-complement/
stackoverflow.com/questions/49331030/bitwise-xor-0xffffffff
