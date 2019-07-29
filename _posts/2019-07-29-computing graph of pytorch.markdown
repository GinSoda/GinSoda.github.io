---
layout: post
title:  "pytorch 反向传播图 计算图"
date:   2019-07-29 13:06:00
categories: Deep_Learning
tags: pytorch computing_graph
mathjax: true
---

* content
{:toc}
介绍一些pytorch内部反向传播图的计算和内部实现

阅读本文前请先观看 https://www.youtube.com/watch?v=MswxJw-8PvE 内容



## 计算图概念
Pytorch的核心功能是自动求导机制即反向传播图/计算图，然而给人的感觉却非常别扭
一是反向传播图在程序中并非一个实体（一个类/对象），而是由众多元素和内部运算机制组成的抽象概念。
二是自动求导过程时非常的别扭和不直观，

```python
import torch  

a = torch.tensor(2.0, requires_grad=True)  
b = torch.tensor(3.0, requires_grad=True)  
c = a + b  
d = torch.tensor(4.0, requires_grad=True)  
e = c * d  

e.backward() # 只有标量scalar，才能运行.backward()执行求导  
print(a.grad)  # a.grad 即导数 d(e)/d(a) 的值  
```

输出：tensor(4.)

在OOP里面，你要改变一个对象的状态，一般的做法是引用这个对象本身，给它的property显示的赋值（比如user.age=18)，或者是调用这个对象的方法（user.setAge(18))，让它状态得以改变。而这里的做法是，调用了一个跟它（a）本身看起来没什么关系的对象（e）的方法，结果改变了它的状态。

## 计算图tensor相关属性

| 名称                  | 描述           |
|:----------------------|:------------------------------------------------------------------------------|
| tensor                | 获得该节点的值，即Tensor类型的值 |
| tensor.data           | Variable类遗留属性，有隐患不建议使用 |
| tensor.grad           | 获得该节点处的梯度信息，代表 d final_output/d x |
| tensor. requires_grad | True和False，True代表此变量处需要计算梯度，False代表不需要，默认是False;	只有浮点型tensor才有此属性;	中间节点的requires_grad不能直接更改，始终为True |
| tensor.grad_fn        | 表示变量是不是一个Function类的输出值。若是则grad_fn返回该Function类，否则是None |
| tensor._version       | 表示该tensor in-place计算次数，默认为0。tensor成为Function类输入后，_version要保持不变，否则backward()时会报错 |
| tensor.retains_gard   | True和False，True代表此变量保留梯度，False代表不保留梯度；默认无此属性，中间节点调用tensor.retain_gard()后，产生此属性，并置为True |

## 计算图结构
计算图中有两种节点：Tensor节点和Function节点，Tensor记录运算数据，Function记录运算操作。其中Tensor节点又可以分为叶节点和中间节点两类。

|               | .requires_grad | .grad_fn  | 备注  |
|:------------- |:-------------- |:--------- |:----- |
| 叶子节点      | False          | None      | 生成叶子节点，无计算图 |
| 叶子节点      | False          | 非None    | 不存在 |
| 叶子节点      | True           | None      | 生成中间节点，并创建计算图 |
| 中间节点      | True           | 非None    |       |

叶子节点：1、用户定义的节点（其tensor.grad_fn = None）2、tensor. requires_grad = False的节点。

`注：在神经网络中，h=Wx，每层网络权重W是叶子节点，每层hidden state是中间节点`
	
中间节点：由其他节点计算所得的tensor。

叶子节点产生中间节点：Function类的所有input节点的.requires_grad都是False时，它输出节点的.requires_grad才是False否则为Ture

## 计算图的建立和释放
![](https://github.com/MaHaLo-G/Storage_Public/blob/master/pytorch%20computing%20graph.png?raw=true)
图例说明：
- 黄色代表. requires_grad=False的叶子节点
- 绿色代表. requires_grad=True的叶子节点
- 棕色代表中间节点/非叶子节点
- 蓝色代表反向传播节点(反向传播图)

```python
input = torch.tensor(log(11), requires_grad=True)  
x = 2 * input # x.grad_fn 指向 MulBackward Function  
y = 3 + x # y.grad_fn 指向 AddBackward Function  
z = torch.exp(y) # z.grad_fn 指向 ExpBackward Function 
```

1. 当.requires_grad=Ture的叶子节点进入Function类时，就会在内存中创建计算图（蓝色节点）
2. 任何计算图上的. requires_grad=True标量节点，都可以调用backward()函数
3. x.backward()函数调用后会释放x.grad_fn对应的蓝色节点以及之前的节点，而不会释放x之后的蓝色节点

	例子：y.backward()会释放AddBackward Function和MulBackward Function，而不会改变ExpBackward Function
	
	推论：叶子节点进行backward()不会释放任何蓝色节点，所以叶子节点可以无限次backward()
4. backward()时遇到已经被释放的蓝色节点，则会报错

	例子：y.backward()后运行z.backward()会报错
	
	推论：如果两个tensor分别backward()时，释放的蓝色节点没有交集，则可以运行，不会报错
5. backward(retain_graph=True)代表本次反向传播不释放任何蓝色节点，可以实现对同一蓝色节点的多次利用
6. 多次backward()时，梯度值累加
7. 只有标量scalar才能调用.backward()

## 中间节点的grad
Pytorch默认不会保存中间节点(intermediate variable)的grad，此举是为了节省内存。详见https://discuss.pytorch.org/t/why-cant-i-see-grad-of-an-intermediate-variable/94

实际上由上图可以知道，反向传播过程中grad值不会经过中间节点，而是由Function类到另一个Function类最后抵达叶子节点。

## in-place operation
在 pytorch 中, 有两种情况不能使用 in-place operation（地址不变，数值改写）:
1. 对于 requires_grad=True 的 叶子张量(leaf tensor) 不能使用 in-place operation
2. 对于在 求梯度阶段需要用到的张量 在成为Function类的输入后 不能使用 in-place operation（注意两个限定条件求梯度需要用到 和 成为Function类的输入后，要同时满足），否则_version属性不一致，backward()时会报错

## x.data 与 x.detach()
相同：
- 都和 x 共享同一块数据
- 都和 x 的 计算历史无关
- requires_grad = False

不同：

`x_detach = x.detach() #对x_detach进行in-place操作，会改变x._version属性`

`x_data = x.data #对x_data进行in-place操作，不会改变x._version属性`

这会导致x_data =x.data 在in-place operation 的第二种情况中，更改x.data后造成梯度求导得出错误结果。

## detach()与detach_()
`tensor.detach() # Returns a new Tensor, detached from the current graph. The result will never require gradient. 返回的张量和原张量共享内存. 返回的张量.grad_fn = None, .requires_grad = False`

`tensor.detach_() #将计算图中节点转为叶子节点，将节点.grad_fn = None, .requires_grad = False`

```python
x = torch.tensor(log(3), requires_grad=True)  
y = torch.exp(x)  
z = y * y  
y.detach_()  
z.backward()  
print(x.grad)  # detach_()不会影响计算图的反向传播
```

输出：tensor(18.) 

## 参考目录：
https://www.youtube.com/watch?v=MswxJw-8PvE

http://chriszou.com/2019/05/25/pytorch-autograd-explained.html

https://zhuanlan.zhihu.com/p/38475183

https://www.cnblogs.com/catnip/p/8760780.html

https://blog.csdn.net/u012436149/article/details/78510945
