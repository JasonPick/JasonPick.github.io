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
      
      
        在单核CPU的情况下的IO密集
        
       ![](https://upload-images.jianshu.io/upload_images/19895418-c2955cec5fbacf00?imageMogr2/auto-orient/strip|imageView2/2/w/1080/format/webp)
   
       我们的目的是最大化CPU利用率，不能让CPU处于空闲状态。
       
       
       **线程等待时间所占比例越高，需要越多线程；线程CPU时间所占比例越高，需要越少线程。**
       
       
       
 2.创建多少个线程是合适的？
 
 
  * CPU密集型：
  
  
     理论上 线程数量 = CPU 核数，但是实际上，数量一般会设置为 CPU 核数（逻辑）+ 1
     
     这个多出来的一个是为了当一个线程出现错误或者因为其他原因而暂停的时候，正好有一个后备线程来确保CPU周期不会中断工作
  
  
  * IO密集型：
  
  
    * 对于单核心来说
    
      ```最佳线程数 = (1/CPU利用率) = 1 + (I/O耗时/CPU耗时)```
      
    * 对于多核心来说
    
       ``` 最佳线程数 = CPU核心数 * (1/CPU利用率) = CPU核心数 * (1 + (I/O耗时/CPU耗时))```
 
   
   
3. 牛刀小试


  -#试题1
  
  {:.box-note}
  
  假设要求一个系统的 TPS（Transaction Per Second 或者 Task Per Second）至少为20，然后假设每个Transaction由一个线程完成，
  继续假设平均每个线程处理一个Transaction的时间为4s,问需要多少线程来在1s内处理20个transaction。
  
  
  答：每个线程每4s处理一个transaction，那么每s一个线程处理0.25个TPS，所以需要80个线程数
  
  优化：一般服务器的CPU核数为16或者32，如果有80个线程，那么肯定会带来太多不必要的线程上下文切换开销，这就需要调优了，来做到最佳 balance
  
  
  -#试题2
  
  {:.box-note}
  
  计算操作需要5ms，DB操作需要 100ms，对于一台 8个CPU的服务器，怎么设置线程数呢？


   线程数 = 8 * (1 + 100/5) = 168 (个)


   {:.box-note}
   
   那如果DB的 QPS（Query Per Second）上限是1000，此时这个线程数又该设置为多大呢？
   
   ```
   1s = 1000ms, 当前一个任务需要 5 + 100 = 105ms
   
  那么一个线程每s可以处理 1000/105 个任务
  
  那么168 个线程可以处理 168 * 1000/105 = 1600 QPS
  
  QPS上限为1000，那么线程数目减少为1600/1000 * 168 = 105 个
   
   ```
   
   
   



  
