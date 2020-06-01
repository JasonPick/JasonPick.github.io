---
layout: post
title: 树专题
subtitle: Part 1：二叉树、二叉搜索树
tags: [algorithms, tree]
comments: true
categories: ['algorithms']
---
在这里我们讨论的树有二叉树、二叉搜索树、平衡二叉树、红黑树。其中我们主要给出二叉树的结构，二叉搜索树的相关题目的分析以及针对红黑树的问答题。
## 二叉树

![二叉树图示](https://www.tutorialspoint.com/data_structures_algorithms/images/binary_tree.jpg)
重要的概念
* **高度**：最长路径的边的个数

### 特殊的二叉树

{: .box-note}
 * **Full binary tree满二叉树**：一个节点只能有0或者2个节点
 * **Complete binary tree完全二叉树**：除了最下层全满，最下层所有叶子结点向左边靠拢 


### 完全二叉树
完全二叉树是_堆_的底层实现

堆的时间复杂度表如下

| 操作 | 时间复杂度| 
| :------ |:--- | 
| 找到最小值 | O(1) |
| 删除最小值| O(logn) | 
| 插入新的数 | O(logn)|

一些关于完全二叉树的统计信息

| 名称 | 表示| 
| :------ |:--- | 
| 高度 | O(log(n)) |
| 结点数| 2^(h+1)-1 | 
| 结点i的子结点 | 2i+1 2i+2|
| 结点i的父亲结点 | (i-1)/2|


## 二叉搜索树

**Binary search tree二叉搜索树**：二叉搜索树的所有节点x，x的左子树都小于x，x的右子树都大于x，非常易于查询。

![二叉树图示](https://www.tutorialspoint.com/data_structures_algorithms/images/binary_tree.jpg)

**Tree Node**
~~~
struct node {
   int data;   
   struct node *leftChild;
   struct node *rightChild;
};
~~~

### 二叉搜索树的增删改查
#### 插入元素操作
插入

**Here is some bold text**




## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

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
