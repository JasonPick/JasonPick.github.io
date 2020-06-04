---
layout: post
title: CSAPP 15-213 笔记 Lecture 04
subtitle: 浮点数
tags: [basic,csapp]
comments: true
categories: ['Basic Knowledge']
---
# 浮点数

本次课讨论了浮点数的表示方法，浮点数的加法乘法以及性质。

## 浮点数的表示

**浮点数的一种表示方法**

我们可以使用如下这种方法来表示浮点数

![浮点数的表示](/assets/img/blog/float_representation_1.JPG)

可是这样的表示方法有一些缺点：
1. 只能表示 x/2^k 的数

2. 不能在保证大的表示范围的基础上保持很好的精确度。

由此，我们使用 **浮点数**。所谓浮点数，就是移动小数点的位置，同时保证较大的表示范围和较好的精确度。

### 浮点数的标准表示方法

我们使用如下方法表示浮点数

![浮点数的公式](/assets/img/blog/float_form.JPG)

其中：S 是符号位， M 在\[1.0,2.0]之间，E 是指数位

我们用下图表示浮点数：

![浮点数图示](/assets/img/blog/float_representation_2.JPG)

浮点数有单精度浮点数和双精度浮点数：

* 单精度浮点数

![](/assets/img/blog/float_representation_3.JPG)

* 双精度浮点数

![](/assets/img/blog/double_representation.JPG)

### Normalized Values

条件：exp != 0000...0 && exp != 1111...1

E = Exp - Bias  

其中 EXP 是 exp 的无符号值  
Bias = 2^(k-1) -1 (32bits: 127, 64bits: 1023)  
M = 1.xxxx...x (相应的 frac = xxxx...x)  

举个例子：
![](/assets/img/blog/float_example_1.JPG)

其中 Exp 的取值范围为：
![](/assets/img/blog/float_exp_range.JPG)

### Denormalized Values

条件：exp = 0000...0

E = 1 - Bias

M = 0.xxxx...x

* exp = 0000...0 && frac = 0000...0 表示 0 这个数。+0 and -0

* exp = 0000...0 && frac != 0000...0 表示靠近0的 denormalized values

### Special Values

* exp = 1111...1, frac = 0000...0, 表示无穷大∞

* exp = 1111...1, frac != 0000...0, 表示 NAN (∞ - ∞ 或者 ∞ * 0 )

### Summary 

float表示数据的范围

![](/assets/img/blog/float_num_range.JPG)

靠近0的数分布比较密集，exp每增加1，距离就增加一倍。

## 浮点数的操作
浮点数可以进行加法和乘法。虽然计算机中的浮点数和真的浮点数很相似，但是计算机中的浮点数还是与真的浮点数不同。

浮点数的加法和乘法操作如果超出了范围则需要 Round() 操作来适合范围。其中 Round 可以是向0取值、向-∞取值、向∞取值和偶数取值。

### 浮点数的乘法
先相乘再进行fit操作

相乘操作

{:.box-note}
S = S1 ^ S2  
M = M1 * M2  
E = E1 + E2  

fit操作

{:.box-note}
M >= 2,向右移动 M 使得 M 在 0.0，1.0，2.0之间   
E 超出来范围，那么就是溢出   
Round(M) 来适应范围。

### 浮点数的加法
先对齐再相加

浮点数的加法不满足结合律

浮点数的乘法不满足结合律和分配律

尤其是当两个数的差值巨大的时候，大数与小数向加为大数。

### cast 操作
对于 int 和 unsigned 进行cast的时候，我们并没有改变数的本身，而是改变我们看待它的方式。

对于 int, double, float 进行cast会改变数的值

double/float -> int:  
* float: 23 bits 可以直接截去 frac 的部分  
* double: 52 bits rounding to fit int  


int -> double/float:  
* float: 先 round 一下
* double: 直接转换  

### 课后练习

![](/assets/img/blog/float_exercise.JPG)


