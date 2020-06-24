---
layout: post
title: 操作系统深入理解
subtitle: 问题整理
cover-img: /assets/img/path.jpg
tags: [OS]
categories: ['Operating System']
---

# 操作系统深入理解


这篇博客是对于操作系统更深层次的理解，通过问题的形式让我更加了解操作系统到底是怎样运行的，为什么要这样做。


-## #关于多线程问题[inspired by this blog](https://www.jianshu.com/p/f30ee2346f9f)


1. 为什么要使用多线程


   既然使用多线程还要承受并发错误的风险，我们为什么要使用它呢？ 答案是使用多线程效率更高，但是这种回答的context并不具体。
   
   
   * 并发编程真的适用于所有程序吗？
   
   * 它为何这么快？
   
   
   {:box-warning}
   
   使用多线程是要在**正确的场景下**，设置正确的线程个数，目的在于最大化程序的运行速度。充分利用 CPU 和 IO
   
   
   * 对于具体的应用场景，我们可以分为两个类别
   
   
      * **CPU密集型程序**
      
      
        🌰例子：假设要计算 '1+2+....+10000' 的和,那么假如单核CPU下创建4个线程的话那么，运行的过程如下：
        
        ![](https://upload-images.jianshu.io/upload_images/19895418-8a4d3c815c2abdb1?imageMogr2/auto-orient/strip|imageView2/2/w/1080/format/webp)
        
        所有的线程都要等待时间片，在这个情况下，四个线程并没有比一个线程更快。
        
        
        {:.box-error}
        
        单核CPU密集型程序不适合使用多线程来处理
        
        
        反之四核CPU下，分为四个线程来计算，会大大提升效率
        
        ![](https://upload-images.jianshu.io/upload_images/19895418-7370e52c09df4d86?imageMogr2/auto-orient/strip|imageView2/2/w/1080/format/webp)
        
        
        {:.box-waring}
        
        **所以，如果是多核CPU 处理 CPU 密集型程序，我们完全可以最大化的利用 CPU 核心数，应用并发编程来提高效率**
      
      
      * **IO密集型程序**
   
   
   
   

  
