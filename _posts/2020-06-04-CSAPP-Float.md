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

### 浮点数的一种表示方法

我们可以使用如下这种方法来表示浮点数

![浮点数的表示](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/img/float_representation_1.jpeg)

可是这样的表示方法有一些缺点：
1. 只能表示 x/2^k 的数

2. 不能在保证大的表示范围的基础上保持很好的精确度。

由此，我们使用 **浮点数**。所谓浮点数，就是移动小数点的位置，同时保证较大的表示范围和较好的精确度。

### 浮点数的标准表示方法

我们使用如下方法表示浮点数

![浮点数的公式](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/img/float_form.jpeg)

其中：S 是符号位， M 在\[1.0,2.0]之间，E 是指数位

我们用下图表示浮点数：

![浮点数图示](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/img/float_representation_2.jpeg)



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

