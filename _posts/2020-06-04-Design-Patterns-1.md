---
layout: post
title: Sample blog post
subtitle: Each post also has a subtitle
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
categories: ['Basic Knowledge']
---

## 观察者模式

观察者模式适用于当一个对象改变时，需要很多其他的对象接受更新（例如股票）。有一个 **Subject** 和多个 **Observer**。类之间的关系如下：

![class diagram](https://www.geeksforgeeks.org/wp-content/uploads/o2.png)

特点：
* Observer 和 Subject都是abstract的类，所有需要订阅的observer需要从Observer类里面继承  
* notify() 方法在subject更新的时候，通知observer，Subject维护了一个注册的observer list  
* observer 有register 和 unregister操作，用于添加和移除observer。  


* 优点：提供了一种松耦合的设计。松耦合是指相互通信的两个对象，应该具有对方的最少信息。松耦合设计对于改变一些要求是非常灵活的。  
* 缺点：当一个observer不再订阅相关信息，但是unregister失败的时候，会发生**Lapsed listener problem**，导致内存泄漏。  

## 工厂模式

工厂模式适用于，当想要返回共享一个父类的多个子类种的一个的时候(例如：星巴克的各种各样的咖啡)。当事先不知道要用哪一个类的时候，选择工厂模式。

![class diagram](https://media.geeksforgeeks.org/wp-content/uploads/20200427212325/Class-Diagram-12.png)

~~~
an example

Consider you have opened a Book shop in a Village. Now the villagers don’t really know the exact names of books or authors. They just know the subject & they know what is the degree of depth they want in that book.

Suppose we have books of following subjects:
1. English
2. Hindi
3. Science

Now each subject can have say, 3 types of each books that are:
1. Basic knowledge
2. Medium knowledge
3. High end knowledge

Suppose a user comes to our book shop and asks for an English book which is of Medium knowledge level.
So in coding terms, he just needs to specify :
EnglishBook eb = EnglishBookFactory.getInstance(Level medium) to get the desired book object.
Without having anything to do with who is the author of that book, or what is the name of the book.
That information has been abstracted from the Client.
~~~

## 抽象工厂模式

抽象工厂模式和工厂模式很相像，但是所有东西都封装了，被视为工厂模式的另一层抽象。抽象工厂模式围绕创建其他工厂的超级工厂工作。抽象工厂提供用于创建相关对象或从属对象的族的接口，而不指定其具体类。


![UML](https://media.geeksforgeeks.org/wp-content/uploads/AbstractFactoryPattern-2.png)

工厂模式的流程如下:
![](\assets\img\blog\fatory_pattern.png)


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
