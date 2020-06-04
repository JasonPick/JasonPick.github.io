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


## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.

