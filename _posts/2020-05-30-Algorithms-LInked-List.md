---
layout: post
title: 链表专题
subtitle: Each post also has a subtitle
tags: [algorithms, linked list]
comments: true
categories: ['algorithms']
---

## 链表数据结构


链表是线性数据结构，其中元素不存储在连续的内存位置。链接列表中的元素使用指针链接，如下图所示：
![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2013/03/Linkedlist.png)


简单地说，链接列表由节点组成，其中每个节点包含一个数据字段和指向列表中下一个节点的引用。


**why linked list**

* 与数组相比

数组可用于存储类似类型的线性数据，但数组具有以下限制。

1） 数组的大小是固定的：因此我们必须事先知道元素数的上限。此外，通常，分配的内存等于上限，无论使用情况如何。

2） 在元素数组中插入新元素的成本很高，因为必须为新元素创建空间，并必须移动现有元素的空间。


**链表的优点**

* 动态大小

* 易于插入/删除


**链表的缺点**


* 不允许随机访问。我们必须从第一个节点开始按顺序访问元素。因此，我们不能使用链接列表有效地执行二分搜索

* 列表的每个元素都需要指针的额外内存空间。

* 缓存不友好。由于数组元素是连续位置，因此存放链接列表时不存在的引用位置。

链表通常的结构

~~~
class Node { 
public: 
    int data; 
    Node* next; 
}; 
~~~

**链表的时间复杂度**


Access O(n), Insert O(1), Delete O(1)


**链表的分类**


* circular links:循环链接列表是一个链接列表，其中所有节点都连接到一个圆圈。末尾没有 NULL。循环链接列表可以是单个循环链接列表或双循环链接列表。

* 




### 习题列表
* 链表删除

{: .box-warning}

   1. 移除链表中的元素 [203 Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)      
   2. 从末尾移除第N个结点 [19 Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)    
   3. 从排序的列表里移除重复的元素 [83 Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)      
   4. 移除重复的元素 [82 Remove Duplicates from Sorted List II ](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)    

* 链表反转与旋转

{: .box-warning}

   1. 反转链表 [206 Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)     
   2. 反转链表II [92 Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)      
   3. 成对旋转结点 [24 Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)      
   4. k个一组旋转链表 [25 Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)    
  
* 环

{: .box-warning}

   1.链表环 [141 Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)    
   2.链表环2[142 Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)   

* 链表排序

{: .box-warning}

   1.排序列表 [148 Sort List](https://leetcode.com/problems/sort-list/)    
   2.排序列表反转 [147 Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)    


* 链表操作

{: .box-warning}

   1. 重新排序列表[143 Reorder List](https://leetcode.com/problems/reorder-list/)    
   2. 旋转列表[61 Rotate List](https://leetcode.com/problems/rotate-list/)      
   3. 切分列表[86 Partition List](https://leetcode.com/problems/partition-list/)    
   4. 奇偶链表[328 Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)  
   5. 把链表分成几部分[725 Split Linked List in Parts](https://leetcode.com/problems/split-linked-list-in-parts/)    
   6. 回文链表[234 Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)      

* 进位加法

{: .box-warning}

   1. 两数之和[2 Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)    
   2. 两数之和2[445 Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)    

* 链表合并

{: .box-warning}

   1. 合并两个排序列表[21 Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)    
   2. 合并k个排序列表[23 Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)    
   3. 两个链表相交[160 Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)    

* 其他

{: .box-warning}

   1. 随机指针拷贝列表[138 Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)   
   2. 把排序列表转化为BST[109 Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)    



This is a demo post to show you how to write blog posts with markdown.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](https://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

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

