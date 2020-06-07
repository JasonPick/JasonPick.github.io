---
layout: post
title: 队列专题
subtitle: Part 1：栈和队列
tags: [algorithms, queue]
comments: true
categories: ['algorithms']
---

在这里我们讨论队列，因为栈和队列的相似性，我们将在这一部分将它们对比讨论。在下一部分我们将讨论优先队列。

# 1. 队列

一个队列是一个线性结构，并且遵循 **先进先出(FIFO)** 的顺序。  

队列的栈的区别在于队列移除最远距离添加的，栈移除最近添加的。


**队列的操作**

队列中主要存在四种操作：

Enqueue:加入一个元素进队列。如果队列已满则会发生溢出。 

Dequeue:从队列中移除一个元素，元素出队的顺序和元素入队的顺序相同。如果队列为空，则会发生underflow。 

Front:获取队首元素。

Rear:获取队尾元素。

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2014/02/Queue.png)

**队列的应用**

队列经常应用于不需要立即处理，并且要求FIFO顺序的事情(BFS)。
1.当一个资源是由多个消费者共享的时候(CPU的分配，disk的分配)
2.当数据是异步的时候(IO缓存，文件IO和管道)

**队列的array实现**

对于实现，主要实现队列的四个操作，跟踪两个index，rear and front。我们不可以直接增加front或者rear，我们采用循环的方法创建queue

```c++
#include <iostream>
using namespace std;

class Queue{
public:
    int front,rear,size;
    unsigned capacity;
    int* array;
};

Queue* createQueue(unsigned capacity){
    Queue* queue = new Queue();
    queue->capacity = capacity;
    queue->front = queue->size = 0;

    //important*********************
    queue->rear = capacity-1;
    queue->array = new int(queue->capacity*sizeof(int));
    return queue;

}
//Queue is full when size = capacity
int isFull(Queue* queue){
    return (queue->size == queue->capacity);
}
int isEmpty(Queue* queue){
   return (queue->size==0);
}
//add item to a queue
void enque(Queue* queue, int item){
    if(isFull(queue))
        return ;
    queue->rear = (queue->rear+1)%queue->capacity;
    queue->array[queue->rear] = item;
    queue->size = queue->size +1;
    cout << item <<"  enqueued to queue"<<endl;
}
// remove an item from queue
int deque(Queue* queue){
    if(isEmpty(queue))
        return INT_MIN;
    int item =  queue->array[queue->front];
    queue->front = (queue->front+1)%queue->capacity;
    queue->size = queue->size- 1;
    return item;
}
//get the front item
int front(Queue* queue){
    if(isEmpty(queue)){
        return INT_MIN;
    }
    return queue->array[queue->front];
}
//get the rear item
int rear(Queue* queue){
    if(isEmpty(queue)){
        return INT_MIN;
    }
    return queue->array[queue->rear];
}
int main(){
    Queue* queue = createQueue(1000);
    enque(queue,10);
    enque(queue,20);
    enque(queue,30);
    enque(queue,40);
    cout<<deque(queue)<<"  deque from queue\n";
    cout<<front(queue)<<"  front item of queue"<<endl;
    cout<<rear(queue)<<"   rear item of queue"<<endl;
    return 0;

}

```

array实现的queue虽然简单，但是却是固定size的。

**队列的linked list实现**

```c++
#include<iostream>

using namespace std;

struct QNode{
    int data;
    QNode* next;
    QNode(int d):data(d),next(nullptr){}
};

struct Queue{
    QNode* front, *rear;
    Queue(){
        front = rear = nullptr;
    }

void enque(int x){
    QNode* node = new QNode(x);
    //empty queue
    if(rear == nullptr){
       rear = front = node;
       return ;
    }
    rear->next = node;
    rear = node;
}


void deque(){
    if(front == nullptr){
        return;
    }
    QNode* node = front;
    front = front->next;

    if(front == nullptr){
        rear = nullptr;
    }
    delete(node);
}

};
int main(){
    Queue q;
    q.enque(10);
    q.enque(20);
    q.deque();
    q.deque();
    q.enque(30);
    q.enque(40);
    q.enque(50);
    q.deque();
    cout<<"Queue Front "<<(q.front)->data<< endl;
    cout<< "Queue Rear "<<(q.rear)->data<<endl;
}

```

**STL中的queue API**

| API | Function |
| :------ |:--- | 
| empty() | Returns whether the queue is empty |
| size() |Returns the size of the queue | 
| swap() | Swap the contents of two same queue | 
| emplace() | Insert a new element into the queue at the end of the queue| 
| front() |  returns a reference to the first element of the queue | 
| back() | returns a reference to the last element of the queue | 
| push(g) |  adds the element ‘g’ at the end of the queue| 
| pop() |  deletes the first element of the queue | 




**2.2 队列习题列表**



* 栈  


常见栈的应用场景包括括号问题的求解，表达式的转换和求值，函数调用和递归实现，深度优先搜索遍历等；  
常见的队列的应用场景包括计算机系统中各种资源的管理，消息缓冲器的管理和广度优先搜索遍历等。  


{: .box-warning}

1. 判断括号是否合法[20 Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)  
2. 最长合法的括号[32 Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)  
3. 用两个栈实现队列[232 Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)  
4. 用两个队列实现栈[225 Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)  
5. 最小栈[155 Min Stack](https://leetcode.com/problems/min-stack/)  
6. 逆波兰表达式求值[150 Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)  
7. 包含加减运算的表达式计算[224 Basic Calculator](https://leetcode.com/problems/basic-calculator/)  
8. 简化Unix目录路径[71 Simplify Path](https://leetcode.com/problems/simplify-path/)  
9. 直方图的最大面积[84 Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)  
10. 去除重复字符[16 Remove Duplicate Letters](https://leetcode.com/problems/3sum-closest/)  
11. 栈的压入、弹出序列：判断栈的压栈、弹栈序列是否合法[《剑指》面试题22](https://www.nowcoder.com/questionTerminal/d77d11405cc7470d82554cb392585106)   



* 其他


{:.box-warning}

1. LRU缓存的实现[](https://leetcode.com/problems/lru-cache/)  



20 合法的括号

思路：
  
非常符合栈的结构，我们遇到左括号将相应的右括号全部压入栈中，如果遇到右括号判断当前栈是否为空或者栈顶元素是否等于当前元素，如果不是那么返回false，最后循环结束，判断一下栈是否为空


32.最长的合法括号

思路：

还是同上题一样，用栈来做，这次，我们用index压入栈中，具体例子如下。
![](/assets/img/blog/Longest_valid_parenthese.JPG)


232.225.用栈实现队列 用队列实现栈

思路：  

 栈实现队列，两个栈一个用于输入一个用于输出。  
 队列实现栈，使用一个队列实现栈，在压入队列之后对整个队列进行反转。  


155.最小栈

思路：

使用两个stack的话，就一个stack负责保存最小值，如果push的值小于最小值或者s2为空，那么push进s2，如果pop的值为最小值，那么s2也pop。  
如果使用一个栈的话，单独加一个最小值变量，如果push的值小于最小值那么，stack push之前的最小值，保留新的最小值在外面，如果pop的值为最小值，那么改变最小值，再pop一次。  


150.逆波兰表达式

思路：

显示的使用栈：从左向右像扫描，如果遇到数字push进satck中，如果遇到operator就pop，pop出的第一个数是y，第二个数是x，x operator y，再把结果push进stack中区


224.简单计算器 hard

思路：
![](https://github.com/JasonPick/JasonPick.github.io/blob/master/assets/img/blog/basic_calculator.JPG)



* 递归：层序遍历和自底向上遍历使用递归，一个helper函数，传入res, level, node， 边界条件node为空return，如果level>=res.size那么在res中加入新的行，将当前val加入res\[level\]中，左循环，右循环。如果是自底向上的遍历，在返回之前对res进行reverse。  
* 循环：之字形遍历使用循环遍历，创造一个queue，将root加入queue中，while循环queue不为空的时候，定义一个新的vector存储这一层的值,弹出queue.front(),queue pop一下，如果flag==0从尾部插入，如果flag==1从头部插入，左子结点加入queue，右子结点加入queue。

**Solutions**



[20 Valid Parentheses](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/20.valid_parentheses.cpp)          
[32 Longest Valid Parentheses](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/32.longest_valid_parentheses.cpp)    
    
[232 Implement Queue using Stacks](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/232.implement_queue_using_stack.cpp)     
    
[225 Implement Stack using Queues](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/22.implement_stack_using_queue.cpp)   
    
[155 Min Stack](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/155.min_stack.cpp)   
      
[150 Evaluate Reverse Polish Notation](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/150.Reverse_polish_.formula.cpp)   
    
[224 Basic Calculator](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/224.basic_calculator.cpp)         
[71 Simplify Path](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/224.basic_calculator.cpp)       
      
[84 Largest Rectangle in Histogram](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/224.basic_calculator.cpp)       
      
[16 Remove Duplicate Letters](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/224.basic_calculator.cpp)       
  
[《剑指》面试题22](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/queue/224.basic_calculator.cpp)       

 

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
