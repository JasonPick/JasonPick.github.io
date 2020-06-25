---
layout: post
title: 大数据探索
subtitle: Part I：初探
cover-img: /assets/img/path.jpg
tags: [bigdata]
categories: ['DataBase']
---

这一篇是对于大数据方面知识的整理，会做成一个系列。


## 大数据技术的发展


大数据的三架马车：分布式文件系统GFS、大数据分布式计算框架MapReduce、NoSQL数据库系统BigTable


Hadoop家族：

FaceBook 开发了Hive使用SQL来转化成Hadoop

其中HBase是从Hadoop中分离出来的、基于HDFS的NoSQL系统。

2012年，Yarn成为一个独立的项目开始运营，随后被各类大数据产品支持，成为大数据平台上最主流的资源调度系统。

同样是在2012年，UC伯克利AMP实验室开发的Spark开始崭露头角。

使用MapReduce进行机器学习计算的时候性能非常差，因为机器学习算法通常需要进行很多次的迭代计算，而MapReduce每执行一次Map和Reduce计算都需要重新启动一次作业，带来大量的无谓消耗。

还有一点就是MapReduce主要使用磁盘作为存储介质

一般说来，像MapReduce、Spark这类计算框架处理的业务场景都被称作批处理计算，因为它们通常针对以“天”为单位产生的数据进行一次计算，然后得到需要的结果
，这中间计算需要花费的时间大概是几十分钟甚至更长的时间。因为计算的数据是非在线得到的实时数据，而是历史数据，所以这类计算也被称为大数据离线计算


一般说来，像MapReduce、Spark这类计算框架处理的业务场景都被称作批处理计算，因为它们通常针对以“天”为单位产生的数据进行一次计算，然后得到需要的结果，
这中间计算需要花费的时间大概是几十分钟甚至更长的时间。因为计算的数据是非在线得到的实时数据，而是历史数据，所以这类计算也被称为大数据离线计算

![](https://static001.geekbang.org/resource/image/ca/73/ca6efc15ead7fb974caaa2478700f873.png)

搜索引擎时代 -> 数据仓库时代 -> 数据挖掘时代-> 机器学习时代



PB级别的数据的处理

1.数据存储容量的问题。既然大数据要解决的是数以PB计的数据计算问题，而一般的服务器磁盘容量通常1～2TB，那么如何存储这么大规模的数据呢？

2.数据读写速度的问题。一般磁盘的连续读写速度为几十MB，以这样的速度，几十PB的数据恐怕要读写到天荒地老。

3.数据可靠性的问题。磁盘大约是计算机设备中最易损坏的硬件了，通常情况一块磁盘使用寿命大概是一年，如果磁盘损坏了，数据怎么办？



## HDFS


  - ### #HDFS是如何实现大数据高速、可靠的存储和访问的。
  
  HDFS是在一个大规模分布式服务器集群上，对数据分片后进行并行读写及冗余存储。因为HDFS可以部署在一个比较大的服务器集群上，集群中所有服务器的磁盘都可供HDFS使用，所以整个HDFS的存储空间可以达到PB级容量。
  
  ![HDFS 的架构图](https://static001.geekbang.org/resource/image/65/d7/65efd126cbcf3930a706f64c6e6457d7.jpg)

  * DataNode
  
     负责文件数据的存储和读写操作，HDFS将文件数据分割成若干数据块（Block），每个DataNode存储一部分数据块，这样文件就分布存储在整个HDFS服务器集群中。
     
     
  * NameNode
   
     负责整个分布式文件系统的元数据（MetaData）管理，也就是文件路径名、数据块的ID以及存储位置等信息。
     
     
     和RAID一样，数据分成若干数据块后存储到不同服务器上，可以实现数据大容量存储，并且不同分片的数据可以并行进行读/写操作，进而实现数据的高速访问。你可以看到，HDFS的大容量存储和高速访问相对比较容易实现，但是HDFS是如何保证存储的高可用性呢？
     
     
     HDFS的高可用设计
     
     * 数据存储容错：对于存储在DataNode上的数据块，计算并存储校验和（CheckSum）
     
     * 磁盘容错：如果DataNode监测到本机的某块磁盘损坏，就将该块磁盘上存储的所有BlockID报告给NameNode，NameNode检查这些数据块还在哪些DataNode上有备份，通知相应的DataNode服务器将对应的数据块复制到其他服务器上
     
     * DataNode容错：DataNode会通过心跳和NameNode保持通信，如果DataNode超时未发送心跳，NameNode就会认为这个DataNode已经宕机失效
     
     * NameNode容错：NameNode是整个HDFS的核心，记录着HDFS文件分配表信息，所有的文件路径和数据块存储信息都保存在NameNode，如果NameNode故障，整个HDFS系统集群都无法使用；如果NameNode上记录的数据丢失，整个集群所有DataNode存储的数据也就没用了。
     
     所以，NameNode高可用容错能力非常重要。NameNode采用主从热备的方式提供高可用服务
     
     ![](https://static001.geekbang.org/resource/image/7c/89/7cb2668644c32364beab0b69e60b3689.png)
     
     集群部署两台NameNode服务器，一台作为主服务器提供服务，一台作为从服务器进行热备，两台服务器通过ZooKeeper选举，主要是通过争夺znode锁资源，决定谁是主服务器。
     
     而DataNode则会向两个NameNode同时发送心跳数据，但是只有主NameNode才能向DataNode返回控制信息。

    正常运行期间，主从NameNode之间通过一个共享存储系统shared edits来同步文件系统的元数据信息。

    当主NameNode服务器宕机，从NameNode会通过ZooKeeper升级成为主服务器，并保证HDFS集群的元数据信息，也就是文件分配表信息完整一致。






