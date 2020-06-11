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

 ![](https://media.geeksforgeeks.org/wp-content/uploads/CircularLinkeList.png)
   * 优点：

     *  任何节点都可以是起点。我们可以从任何点开始遍历整个列表。当再次访问第一个访问节点时，我们只需要停止。

     *  可用于实现队列，不需要维护前后两个指针。我们可以维护指向最后一个插入节点的指针，并且始终可以作为最后一个节点获取前面的指针。

     *  循环列表可以反复进入列表，在应用程序中很有用。例如，当多个应用程序在 PC 上运行时，操作系统通常将正在运行的应用程序放在列表中，然后循环浏览它们，为每个应用程序提供一段执行时间，然后在将 CPU 提供给另一个应用程序时让它们等待。操作系统使用循环列表很方便，这样当它到达列表末尾时，它可以循环到列表的前面。

     *  循环双链接列表用于实现高级数据结构，如斐波那契堆。
 
 

* Doubly list 双向链表：双链接列表 （DLL） 包含一个前向指针，以及单独链表中的后向指针指针和数据。

  ![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2014/03/DLL1.png)
  
  * 优点
  
    *  DLL 可以向前和向后遍历。
    
    *  如果给定指向要删除的节点的指针，则 DLL 中的删除操作将更为高效。也可以在给定节点之前快速插入新节点。
    
    *  在单链表中，要删除节点，需要指向前一个节点的指针。要获取此上一个节点，有时将遍历列表。在 DLL 中，我们可以使用前向指针获取上一个节点。





### 习题列表


* dummy 用于可能会删除head的情况

* 双指针法也是经常经常使用的


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



203.给定值删除链表中的元素

思路：


* prev -> next = prev->next->next。

* 注意:删除掉next结点的话就不用prev = prev.next了，删除结点时prev.next发生变化所以不用做什么，只有不删除时才向后移动，否则有可能空指针！


19.从末尾移除第n个结点

思路：
 
 利用双指针定位。先用一个fast指针走n步，然后slow指针与fast指针同步，当fast走到链表末尾的时候，slow指针即为倒数第n个结点。
 
 
83.从排序的列表中去除重复的元素：

思路：


* 第一种向后比较结点，发现重复就删掉。与203一样要注意删掉next结点的话就不用 cur = cur-> next;

* 第二种向前比较结点，prev记录前一个结点的值，相同就删掉当前结点。注意prev和cur的更新 prev = cur， cur = cur->next。


82.移除重复的元素

思路：


与83题不同，题目要求删除所有重复结点，不保留。所以while循环里还要嵌套循环，一旦发现duplicate全部删除。


206 反转链表

思路：

* 迭代方法：用一个 pre保存前一个node，一个cur保存现在的Node,cur->next = pre，并且将pre和cur更新。

* 递归方法：用一个helper函数, 传入head 和 newHead,不断将head->next 指向newhead,并向下递归 helper(nxt,head);


92 反转链表II

思路：


* 先找到要插入的位置m的前一个结点，记录start = prev,end = prev->next;反转链表，将反转后的部分重新连接进入链表中。

* start->next = prev,end->next = curr


举个例子 1->2->3->4->5 m = 2, n = 4

首先找到想要反转的队列的前一个值，我们找到了1，此时prev->1,curr->2;

记录此时的prev为start,记录此时的curr为end。逐一反转m到n

我们重新连接链表 此时反转部分是 4->3->2，其中4是循环后的prev，5是循环后的start。将start->next连接到prev上(1->4),将end->next连接到curr上(2->5)。


24.成对旋转结点

思路：


* 逐一反转，借助dummy，设置一个prev结点，记录当前head为A，head->next为B。是的prev指向B，B->A。继续迭代pre = A，A->next = B->next,head = A->next


25.k个一组旋转结点

思路：


* 辅助变量dummy，prev,end;首先找到第一个kgroup迭代end，如果元素个数小于k那么直接break；start = prev->next,end = end->next.

* 切分链表，[start,end]是我们要反转的区间，end->next = nullptr；逐一反转

* 重新拼接，start->next = nxt, prev->next = reverse(start)

* 新的迭代 prev = start, end = start;新的一轮迭代


141.链表环

思路：


* 双指针法最为直接，一个fast指针走两步，一个slow指针走一步。两个指针如果相遇，那么存在环


142.链表环II

思路：


* 双指针法先判断是否有环，如果有环，初始化另一个指针temp = head;与slow同步走，当二者相遇时候，这时的slow就是入环的起点。


148.链表排序

思路：


* mergesort，首先先将链表分为两部分，采用双指针法，左边头结点是head，尾结点是temp，右边的头结点是slow，尾结点是fast。

* 第二步，合并左右部分。如果左边的值小于右边，curr->next = l,l = l->next否则curr->next = r;如果l不为空或者r不为空，直接连接。



147.链表插入排序

思路：


* 插入排序的步骤，向后遍历，如果有值小于当前值，那么先删除这个结点

* 然后从左向右遍历，找到一个合适的位置插入。


143.

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

