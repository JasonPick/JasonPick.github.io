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

![](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/img/queue_picture.jpeg)

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

**队列的linked list实现**



两个栈实现一个队列

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
