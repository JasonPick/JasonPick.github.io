---
layout: post
title: 树专题
subtitle: Part 1：二叉树、二叉搜索树
tags: [algorithms, tree]
comments: true
categories: ['algorithms']
---
在这里我们讨论的树有二叉树、二叉搜索树、平衡二叉树、红黑树。其中我们主要给出二叉树的结构，二叉搜索树的相关题目的分析以及针对红黑树的问答题。
## 1.二叉树

![二叉树图示](https://www.tutorialspoint.com/data_structures_algorithms/images/binary_tree.jpg)
重要的概念
* **高度**:最长路径的边的个数

### 1.1 特殊的二叉树

{: .box-note}
 **Full binary tree满二叉树**：一个节点只能有0或者2个节点  
 **Complete binary tree完全二叉树**：除了最下层全满，最下层所有叶子结点向左边靠拢   


### 1.2 完全二叉树
完全二叉树是 _堆_ 的底层实现

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

待续

## 2. 二叉搜索树

Binary search tree二叉搜索树：二叉搜索树的所有节点x，x的左子树都小于x，x的右子树都大于x，非常易于查询。

![二叉树图示](https://www.tutorialspoint.com/data_structures_algorithms/images/binary_tree.jpg)

**Tree Node**
~~~
struct node {
   int data;   
   struct node *leftChild;
   struct node *rightChild;
};
~~~

**2.1.1 二叉搜索树的增删改查 - 插入元素**

插入元素，首先创建一个树，找到合适的插入位置。如果要插入的树小于root的值，那么查找左子树的合适位置，否则查找右子树的合适位置。

~~~
void insert(int data) {
   struct node *tempNode = (struct node*) malloc(sizeof(struct node));
   struct node *current;
   struct node *parent;

   tempNode->data = data;
   tempNode->leftChild = NULL;
   tempNode->rightChild = NULL;

   //if tree is empty, create root node
   if(root == NULL) {
      root = tempNode;
   } else {
      current = root;
      parent  = NULL;

      while(1) {                
         parent = current;

         //go to left of the tree
         if(data < parent->data) {
            current = current->leftChild;                
            
            //insert to the left
            if(current == NULL) {
               parent->leftChild = tempNode;
               return;
            }
         }
			
         //go to right of the tree
         else {
            current = current->rightChild;
            
            //insert to the right
            if(current == NULL) {
               parent->rightChild = tempNode;
               return;
            }
         }
      }            
   }
}
~~~

**2.1.2 二叉树的增删改查 - 查找**

从根结点开始搜索，如果data<key 那么查找左子树，否则查找右子树，每个结点都重复这个过程。

~~~
struct node* search(int data) {
   struct node *current = root;
   printf("Visiting elements: ");

   while(current->data != data) {
      if(current != NULL)
      printf("%d ",current->data); 
      
      //go to left tree

      if(current->data > data) {
         current = current->leftChild;
      }
      //else go to right tree
      else {                
         current = current->rightChild;
      }

      //not found
      if(current == NULL) {
         return NULL;
      }

      return current;
   }  
}
~~~

**2.2 二叉树习题列表**

* 二叉树的遍历

{: .box-warning}

 1.前序遍历 [144 Binary Tree Preorder Traversal ](https://leetcode.com/problems/binary-tree-preorder-traversal/)  
 2.中序遍历[94 Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)  
 3.后序遍历[145 Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)  

* 二叉树的层序遍历

{: .box-warning}

 1.层序遍历 [102 Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)  
 2.bottom-up 层序遍历[107 Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)  
 3.之字形层序遍历[103 Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)  
 4.垂直遍历 [314 Binary Tree Vertical Order Traversal]()  
 

144.94.145 二叉树的前序遍历、中序遍历、后序遍历

思路：
* 递归： 构造一个helper函数，将res的引用作为参数传入helper，helper实现功能。  
* 循环： 先构造一个stack，对于前序和后序遍历来说先将root压入栈内，while循环，条件为栈不为空，弹出栈顶元素，如果不为空那么先将val存入res，左结点右结点入栈（如果是先序，那么右结点先入否则左结点先入），如果是后序遍历在结尾处需要反转res，再返回。  
对于中序来说，先不将root压入栈中，cur=root，找cur的最左结点，将cur压入栈中，找到最左之后，弹出栈顶元素，将val加入res，cur=cur->right。





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
