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

一个二叉树结点主要包含三个部分：

1.Data

2.Pointer to left child

3.Pointer to right child

![二叉树图示](https://www.tutorialspoint.com/data_structures_algorithms/images/binary_tree.jpg)

重要的概念

二叉树不像是

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
 
* 二叉树的搜索

{: .box-warning}

1.二叉树的最左下树结点的值[513 Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)  
2.二叉树每一行的最大值[515 Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row)    
3.二叉树中第二小的结点[671 Second Minimum Node In a Binary Tree](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree)  
4.每个结点的右指针[116 Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node)  
5.每个结点的右向指针二[117 Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii)  
6.二叉树中增加一行[623 Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree)  
7.二叉树的层平均值[637 Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree)  
8.二叉树的左叶子结点的和[404 Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves)  
9.打印二叉树每一层的最右结点[199 Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)  
10.二叉树中两个结点的最近公共祖先[236 Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree)  
11.二叉树的上下颠倒[156 Binary Tree Upside Down](hhttps://leetcode.com/problems/binary-tree-upside-down)  
12.合并两个二叉树[617 Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees)  
13.最大值二叉树[654 Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree)  
14.二叉树的坡度[563 Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt)  
15.二叉树的下一个结点[No.58](https://www.nowcoder.com/questionTerminal/9023a0c988684a53960365b889ceaf5e)  
16.把二叉树打印成多行[No.60](https://www.nowcoder.com/questionTerminal/445c44d982d04483b04a54f298796288)  



144.94.145 二叉树的前序遍历、中序遍历、后序遍历

思路：
* 递归： 构造一个helper函数，将res的引用作为参数传入helper，helper实现功能。  
* 循环： 先构造一个stack，对于前序和后序遍历来说先将root压入栈内，while循环，条件为栈不为空，弹出栈顶元素，如果不为空那么先将val存入res，左结点右结点入栈（如果是先序，那么右结点先入否则左结点先入），如果是后序遍历在结尾处需要反转res，再返回。  
对于中序来说，先不将root压入栈中，cur=root，找cur的最左结点，将cur压入栈中，找到最左之后，弹出栈顶元素，将val加入res，cur=cur->right。

**Solutions**

{: .box-error}

1.[144 PreOrder](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/144.binary_tree_preorder.cpp).  
2.[94 Inorder](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/94.binary_tree_inorder.cpp).   
3.[145 PostOrder](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/145.binary_tree_postorder.cpp).   

102.103.107 二叉树的层序遍历、之字形遍历、自底向上遍历

思路：
* 递归：层序遍历和自底向上遍历使用递归，一个helper函数，传入res, level, node， 边界条件node为空return，如果level>=res.size那么在res中加入新的行，将当前val加入res\[level\]中，左循环，右循环。如果是自底向上的遍历，在返回之前对res进行reverse。  
* 循环：之字形遍历使用循环遍历，创造一个queue，将root加入queue中，while循环queue不为空的时候，定义一个新的vector存储这一层的值,弹出queue.front(),queue pop一下，如果flag==0从尾部插入，如果flag==1从头部插入，左子结点加入queue，右子结点加入queue。

**Solutions**

{: .box-error}

1.[102 level order](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/102.binary_tree_level_order.cpp).   
2.[107 bottom_up order](https://github.com/JasonPick/recordings-in Jan/blob/leecode/leecode/tree/107.binary_tree_bottom_up_order.cpp).   
3.[103 zigzag order](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/103.binary_tree_zigzag_order.cpp).   


513.二叉树最左下结点的值

思路：  
本质上是一个遍历问题，我们想要得到最左下结点的值，我们就要先遍历右边，再遍历左边。定义一个queue，push(root),while循环，pop(),push(右边)再push(左边),最后返回root->val.

515.二叉树每一行的最大值

思路：  
本质上是一个层序遍历问题，我们先把每一行的一个值放入res中，如果遇到比他大的则替换。定义一个helper函数，追踪每一层的信息，如果level>=res.size()就push(root->val)，res.at(level) = max(res.at(level),root->val),左递归右递归。 

671.找到二叉树第二小的值

思路：  
根据题中的描述，我们知道一个根结点的值是子结点中的较小值，而且一个结点有0个或者2个子结点。因此我们推断第二小的值一定出现在level1中较小值的子结点和较大值中。我们采用分治的办法处理此问题，如果root为空或者root没有子结点返回-1，left代表左边最小值，如果左子结点和根结点值相等，更新left，否则不更新，右边同理，最后如果left和right里面含有-1那么返回最大值，否则返回最小值。


116.找到每个结点的右向指针：

思路：  
* 递归：采用递归的方法，对于每一个结点来说，有两种结点指针。第一，如果有左孩子，题中描述perfect binary tree那么左孩子必定指向右孩子。第二，如果root->next 不为空，并且root->right也不为空，那么root->right->next = root->next->left  
* 迭代：迭代方法也是同理，只是将递归变为两两层循环。第一层循环是每一层的start node，start = start->left。第二层循环是每一层的各个结点，进行如 _递归_ 中描述的两种结点的处理。

117.找到每个结点的右向指针(普通二叉树)

思路：  
题目总体和116一样，只是变成了普通的二叉树。增加一个prev变量，共有三个变量cur，start和prev。采用两次循环办法，对于外循环cur = start, prev = start =null, 条件是cur!=null。对于内循环，分别处理两种情况，左结点不为空和右结点不为空。当左结点不为空，如果prev也不为空那么prev->next = cur->left,如果prev为空的话，那么start = cur->left,处理完毕prev = cur->left。当右结点不为空的话同理。cur=cur->next循环。 

623.二叉树中增加一行

思路：
* 递归：对于d==1，需要替换root，单独处理，新增一个helper函数。helper函数中root == null为边界条件，对于level<d-1左递归右递归，否则root的左结点替换，右结点替换。
* 迭代：对于d==1，需要替换root，创建一个队列。queue.push(root)，while循环，queue不为空，如果(--d == 0) return root,处理完毕。size = queue.size(),循环，如果d==1,那么替换操作，否则将左子结点右子结点加入queue中。

637.二叉树每一行的平均值

思路：  
* 递归：是一个bfs的简单应用记录下每一行的sum和count，每次循环结束res.push(sum/count)
* 迭代：是一个dfs的简单变体，新增一个vector<int> count,记录每一层的结点数，返回之前先将res每个值除以对应的count  
	
	
**Solutions**

{: .box-error}

1.[513 find bottom left tree value](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/513.find_bottom_left_tree_value.cpp)  
2.[515 find largest value in each row](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/515.find_largest_value_in_each_tree_row.cpp)  
3.[671 second minimum node in binary tree](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/671.second_minimum_node_in_binary_tree.cpp)  
4.[116 populating right pointer in perfect binary tree](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/116.populating_next_rigth_pointer_in_each_node.cpp)  
5.[117 populating right pointer in ordinary binary tree](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/117.populating_next_rigth_pointer_2.cpp)  
6.[623 add new row to binary tree](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/623.add_one_row_to_tree.cpp)  
7.[637 average of every row](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/tree/637.average_of_level.cpp)  

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
