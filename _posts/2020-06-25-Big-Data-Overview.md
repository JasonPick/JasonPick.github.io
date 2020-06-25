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

搜索引擎时代 -> 数据仓库时代->数据
