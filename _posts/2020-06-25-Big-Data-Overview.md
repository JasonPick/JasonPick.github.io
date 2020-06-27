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


大数据的时代始于Google发表的论文，也就是三架马车：分布式文件系统GFS、大数据分布式计算框架MapReduce、NoSQL数据库系统BigTable


在这个基础之上Hadoop出现并流行起来


随后**Hadoop家族**开始壮大发展，Hadoop的周边产品不断涌现：


  -为了更加简便的使用，FaceBook 开发了Hive使用SQL语言来转化成Hadoop可以识别的


  -HBase是从Hadoop中分离出来的、基于HDFS的NoSQL系统。


  -Yarn成为大数据平台上最主流的资源调度系统。
  
  
**Spark的兴起**


在2012年，UC伯克利AMP实验室开发的Spark开始崭露头角。


Spark兴起的原因：


  * 使用MapReduce进行机器学习计算的时候性能非常差，因为机器学习算法通常需要进行很多次的迭代计算，而MapReduce每执行一次Map和Reduce计算都需要重新启动一次作业，带来大量的无谓消耗。

  * 还有一点就是MapReduce主要使用磁盘作为存储介质
  


#### 大数据计算的分类

* 大数据离线计算：

  一般说来，像MapReduce、Spark这类计算框架处理的业务场景都被称作批处理计算，因为它们通常针对以“天”为单位产生的数据进行一次计算，然后得到需要的结果
  ，这中间计算需要花费的时间大概是几十分钟甚至更长的时间。因为计算的数据是非在线得到的实时数据，而是历史数据，所以这类计算也被称为大数据离线计算


* 大数据实时计算

  而在大数据领域，还有另外一类应用场景，它们需要对实时产生的大量数据进行即时计算，比如对于遍布城市的监控摄像头进行人脸识别和嫌犯追踪。这类计算称为大数据流计算，相应地，有Storm、Flink、Spark Streaming等流计算框架来满足此类大数据应用的场景。 流式计算要处理的数据是实时在线产生的数据，所以这类计算也被称为大数据实时计算。


![](https://static001.geekbang.org/resource/image/ca/73/ca6efc15ead7fb974caaa2478700f873.png)



大数据经历了以下时代：


搜索引擎时代 -> 数据仓库时代 -> 数据挖掘时代-> 机器学习时代



大数据时代中大量的数据不断产生：一般是PB级别的数据的处理


对于PB级别的数据处理我们要解决三个问题：


1.数据存储容量的问题。既然大数据要解决的是数以PB计的数据计算问题，而一般的服务器磁盘容量通常1～2TB，那么如何存储这么大规模的数据呢？

2.数据读写速度的问题。一般磁盘的连续读写速度为几十MB，以这样的速度，几十PB的数据恐怕要读写到天荒地老。

3.数据可靠性的问题。磁盘大约是计算机设备中最易损坏的硬件了，通常情况一块磁盘使用寿命大概是一年，如果磁盘损坏了，数据怎么办？



## HDFS


  - ### HDFS是如何实现大数据高速、可靠的存储和访问的。
  
  
  HDFS是在一个大规模分布式服务器集群上，**对数据分片后进行并行读写及冗余存储**。因为HDFS可以部署在一个比较大的服务器集群上，集群中所有服务器的磁盘都可供HDFS使用，所以整个HDFS的存储空间可以达到PB级容量。
  
  
  ![HDFS 的架构图](https://static001.geekbang.org/resource/image/65/d7/65efd126cbcf3930a706f64c6e6457d7.jpg)
  
  
  HDFS主要有两个组成部分：


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
     
     
     -集群部署两台NameNode服务器，一台作为主服务器提供服务，一台作为从服务器进行热备，两台服务器通过ZooKeeper选举，主要是通过争夺znode锁资源，决定谁是主服务器。
     
     -而DataNode则会向两个NameNode同时发送心跳数据，但是只有主NameNode才能向DataNode返回控制信息。

     -正常运行期间，主从NameNode之间通过一个共享存储系统shared edits来同步文件系统的元数据信息。

     -当主NameNode服务器宕机，从NameNode会通过ZooKeeper升级成为主服务器，并保证HDFS集群的元数据信息，也就是文件分配表信息完整一致。



- ### HDFS的存储机制
  
  
* 写操作


```
1.客户端向NameNode发出写文件请求。

2.检查是否已存在文件、检查权限。若通过检查，直接先将操作写入EditLog，并返回输出流对象。

（注：WAL，write ahead log**，先写Log，再写内存**，因为EditLog记录的是最新的HDFS客户端执行所有的写操作。如果后续真实写操作失败了，由于在真实写操作之前，操作就被写入EditLog中了，故EditLog中仍会有记录，我们不用担心后续client读不到相应的数据块，因为在第5步中DataNode收到块后会有一返回确认信息，若没写成功，发送端没收到确认信息，会一直重试，直到成功）

3.client端按128MB的块切分文件。

4.client将NameNode返回的分配的可写的DataNode列表和Data数据一同发送给最近的第一个DataNode节点，此后client端和NameNode分配的多个DataNode构成pipeline管道，client端向输出流对象中写数据。client每向第一个DataNode写入一个packet，这个packet便会直接在pipeline里传给第二个、第三个…DataNode。

客户端给DataNode发数据实际上是以Packet的形式发送的，Packet一般只有64KB左右

（注：并不是写好一个块或一整个文件后才向后分发）

5.每个DataNode写完一个块后，会返回确认信息。

（注：并不是每写完一个packet后就返回确认信息，个人觉得因为packet中的每个chunk都携带校验信息，没必要每写一个就汇报一下，这样效率太慢。正确的做法是写完一个block块后，对校验信息进行汇总分析，就能得出是否有块写错的情况发生）

6.写完数据，关闭输输出流。

7.发送完成信号给NameNode。

（注：发送完成信号的时机取决于集群是强一致性还是最终一致性，强一致性则需要所有DataNode写完后才向NameNode汇报。最终一致性则其中任意一个DataNode写完后就能单独向NameNode汇报，HDFS一般情况下都是强调强一致性）

```



* 读操作

```
1）客户端向namenode请求下载文件，namenode通过查询元数据，找到文件块所在的datanode地址。

2）挑选一台datanode（就近原则，然后随机）服务器，请求读取数据。

3）datanode开始传输数据给客户端（从磁盘里面读取数据放入流，以packet为单位来做校验）。

4）客户端以packet为单位接收，先在本地缓存，然后写入目标文件。

```


- ## MapReduce


MapReduce既是一个编程模型又是一个计算框架


![](https://static001.geekbang.org/resource/image/55/ba/5571ed29c5c2254520052adceadf9cba.png)


MapReduce编程模型：包含 Map 和 Reduce 两个过程，Map阶段给每个数据块分配一个map，key相同的数据分配给同一个Reduce，Reduce阶段进行count处理


### MapReduce计算框架是如何运作的


有两个关键性问题：


* 如何为每个数据块分配一个Map计算任务，也就是代码是如何发送到数据块所在服务器的，发送后是如何启动的，启动以后如何知道自己需要计算的数据在文件什么位置（BlockID是什么）。


* 处于不同服务器的map输出的<Key, Value> ，如何把相同的Key聚合在一起发送给Reduce任务进行处理。


我们以Hadoop 1为例，MapReduce运行过程涉及三类关键进程。


1.大数据应用进程。这类进程是启动MapReduce程序的主入口，主要是指定Map和Reduce类、输入输出文件路径等，并提交作业给Hadoop集群，也就是下面提到的JobTracker进程。这是由用户启动的MapReduce程序进程，比如我们上期提到的WordCount程序。

2.JobTracker进程。这类进程根据要处理的输入数据量，命令下面提到的TaskTracker进程启动相应数量的Map和Reduce进程任务，并管理整个作业生命周期的任务调度和监控。这是Hadoop集群的常驻进程，需要注意的是，JobTracker进程在整个Hadoop集群全局唯一。

3.TaskTracker进程。这个进程负责启动和管理Map进程以及Reduce进程。因为需要每个数据块都有对应的map函数，TaskTracker进程通常和HDFS的DataNode进程启动在同一个服务器。也就是说，Hadoop集群中绝大多数服务器同时运行DataNode进程和TaskTracker进程。

JobTracker进程和TaskTracker进程是主从关系，主服务器只有一台，掌控全局；从服务器很多台，负责具体的事情。

第一个问题：


![](https://static001.geekbang.org/resource/image/2d/27/2df4e1976fd8a6ac4a46047d85261027.png)


第二个问题:数据的合并与连接


![](https://static001.geekbang.org/resource/image/d6/c7/d64daa9a621c1d423d4a1c13054396c7.png)


#### shuffle机制


每个Map任务的计算结果都会写入到本地文件系统，等Map任务快要计算完成的时候，MapReduce计算框架会启动shuffle过程，


在Map任务进程调用一个Partitioner接口，对Map产生的每个<Key, Value>进行Reduce分区选择，然后通过HTTP通信发送给对应的Reduce进程。


这样不管Map位于哪个服务器节点，相同的Key一定会被发送给相同的Reduce进程。Reduce任务进程对收到的<Key, Value>进行排序和合并，相同的Key放在一起，组成一个<Key, Value集合>传递给Reduce执行。


map输出的<Key, Value>shuffle到哪个Reduce进程是这里的关键，它是由Partitioner来实现，MapReduce框架默认的Partitioner用Key的哈希值对Reduce任务数量取模，相同的Key一定会落在相同的Reduce任务ID上



**Shuffle 过程分为Map端跟Reduce端的Shuffle过程**


* Map端流程


* 环形内存缓存区：每个split数据交由一个map任务处理，map的处理结果不会直接写到硬盘上，会先输送到环形内存缓存区中，默认的大小是100M（可通过配置修改），当缓冲区的内容达到80%后会开始溢出，此时缓存区的溢出内容会被写到磁盘上，形成一个个spill file，注意这个文件没有固定大小。


* 在内存中经过分区、排序后溢出到磁盘：分区主要功能是用来指定 map 的输出结果交给哪个 reduce 任务处理，默认是通过 map 输出结果的 key 值取hashcode 对代码中配置的 redue task数量取模运算，值一样的分到一个区，也就是一个 reduce 任务对应一个分区的数据。这样做的好处就是可以避免有的 reduce 任务分配到大量的数据，而有的 reduce 任务只分配到少量甚至没有数据，平均 reduce 的处理能力。并且在每一个分区（partition）中，都会有一个 sort by key 排序，如果此时设置了 Combiner，将排序后的结果进行 Combine 操作，相当于 map 阶段的本地 reduce，这样做的目的是让尽可能少的数据写入到磁盘。


* 合并溢出文件：随着 map 任务的执行，不断溢出文件，直到输出最后一个记录，可能会产生大量的溢出文件，这时需要对这些大量的溢出文件进行合并，在合并文件的过程中会不断的进行排序跟 Combine 操作，这样做有两个好处：减少每次写入磁盘的数据量&减少下一步 reduce 阶段网络传输的数据量。最后合并成了一个分区且排序的大文件，此时可以再进行配置压缩处理，可以减少不同节点间的网络传输量。合并完成后着手将数据拷贝给相对应的reduce 处理。


* Reduce端流程


reduce 会接收到不同 map 任务传来的有序数据，如果 reduce 接收到的数据较小，则会存在内存缓冲区中，直到数据量达到该缓存区的一定比例时对数据进行合并后溢写到磁盘上。随着溢写的文件越来越多，后台的线程会将他们合并成一个更大的有序的文件，可以为后面合并节省时间。这其实跟 map端的操作一样，都是反复的进行排序、合并，这也是 Hadoop 的灵魂所在，但是如果在 map 已经压缩过，在合并排序之前要先进行解压缩。合并的过程会产生很多中间文件，但是最后一个合并的结果是不需要写到磁盘上，而是可以直接输入到 reduce 函数中计算，每个 reduce 对应一个输出结果文件。


### YARN的实现原理


Yarn资源调度框架


* **WHY ？** 


  在MapReduce应用程序的启动过程中，最重要的就是要把MapReduce程序分发到大数据集群的服务器上，在Hadoop 1中，这个过程主要是通过TaskTracker和JobTracker通信来完成。

  
  这个方案有什么缺点吗？


  这种架构方案的主要缺点是，服务器集群资源调度管理和MapReduce执行过程耦合在一起，如果想在当前集群中运行其他计算任务，比如Spark或者Storm，就无法统一使用集群中的资源了。
  
  
* **WHAT ?**

![](https://static001.geekbang.org/resource/image/af/b1/af90905013e5869f598c163c09d718b1.jpg)

从图上看，Yarn包括两个部分：一个是资源管理器（Resource Manager），一个是节点管理器（Node Manager）。

这也是Yarn的两种主要进程：


  * ResourceManager进程负责整个集群的资源调度管理，通常部署在独立的服务器上；
  
  
  * NodeManager进程负责具体服务器上的资源和任务管理，在集群的每一台计算服务器上都会启动，基本上跟HDFS的DataNode进程一起出现



具体说来，资源管理器又包括两个主要组件：调度器和应用程序管理器。


  * 调度器： 是一个资源分配算法，根据应用程序（Client）提交的资源申请和当前服务器集群的资源状况进行资源分配。
  
     Yarn进行资源分配的单位是容器（Container），每个容器包含了一定量的内存、CPU等计算资源，Yarn进行资源分配的单位是容器（Container），
     
     容器由NodeManager进程启动和管理，NodeManger进程会监控本节点上容器的运行状况并向ResourceManger进程汇报。
  
  
  * 应用程序管理器 ：负责应用程序的提交、监控应用程序运行状态等。
    
    应用程序启动后需要在集群中运行一个ApplicationMaster，ApplicationMaster也需要运行在容器里面。
    
    每个应用程序启动后都会先启动自己的ApplicationMaster，由ApplicationMaster根据应用程序的资源需求进一步向ResourceManager进程申请容器资源，得到容器以后就会分发自己的应用程序代码到容器上启动，进而开始分布式计算。
    
    
 **Yarn的流程**
  
  ```
 1.我们向Yarn提交应用程序，包括MapReduce ApplicationMaster、我们的MapReduce程序，以及MapReduce Application启动命令。

2.ResourceManager进程和NodeManager进程通信，根据集群资源，为用户程序分配第一个容器，并将MapReduce ApplicationMaster分发到这个容器上面，并在容器里面启动MapReduce ApplicationMaster。

3.MapReduce ApplicationMaster启动后立即向ResourceManager进程注册，并为自己的应用程序申请容器资源。

4.MapReduce ApplicationMaster申请到需要的容器后，立即和相应的NodeManager进程通信，将用户MapReduce程序分发到NodeManager进程所在服务器，并在容器中运行，运行的就是Map或者Reduce任务。

5.Map或者Reduce任务在运行期和MapReduce ApplicationMaster通信，汇报自己的运行状态，如果运行结束，MapReduce ApplicationMaster向ResourceManager进程注销并释放所有的容器资源。
  
  ```
  


### Hadoop 1.0 和 Hadoop2.0 的区别：

Hadoop1.0 和 Hadoop2.0 的架构如下图：


![](https://img2018.cnblogs.com/blog/1011838/201812/1011838-20181214213729657-95804605.png)


Hadoop2.0比起Hadoop1.0来说，最大的改进是加入了资源调度框架Yarn。


* 从HDFS的角度来说
   
   
   -Hadoop1的HDFS存在一些缺点：NameNode的单点故障问题，NameNode的拓展性问题，性能问题（当HDFS存在大量小文件时候）和隔离性问题（一个用户操作很大的job影响slow其他用户的job）
   
   -Hadoop2作出一些改进，提出HDFSFederation以及高可用HA。
   
     -HDFSFederation使得NameNode间相互独立，也就是说它们之间不需要相互协调。且多个NameNode分管不同的目录进而实现访问隔离和横向扩展。
     
     -高可用(HA),HA主要指的主从热备。这样，当一个NameNode所在的服务器宕机时，可以在数据不丢失的情况下，手工或者自动切换到另一个NameNode提供服务。


* MapReduce：针对Hadoop1.0中MR的不足，引入了Yarn框架。Yarn框架中将JobTracker资源分配和作业控制分开，分为Resource Manager(RM)以及Application Master(AM)。


总的来说，Hadoop1 和 Hadoop2的区别可以简化

* Hadoop1：我收到任务，由我来拆解任务成子任务，并且联系下方的同学谁有空就谁做，有任务的同学单独向我汇报进度。

* Hadoop2:我知道有个任务要处理，在同学中选中一位班长，由班长来拆解子任务并告诉我需要的多少同学去做，由班长收集进度并汇报给我。




## Hive


#### MapReduce实现SQL的原理

SQL语句

'SELECT pageid, age, count(1) FROM pv_users GROUP BY pageid, age;'

![](https://static001.geekbang.org/resource/image/0a/37/0ade10e49216575962e071d6fe9a7137.jpg)


* map函数的输入输出

  -输入：Key和Value，Value就是左边表中每一行的数据，比如<1, 25>
  
  -输出：以输入的Value作为Key，Value统一设为1，比如<<1, 25>, 1>
  
  
* shuffle函数

  - 相同的Key及其对应的Value被放在一起组成一个<Key, Value集合>
  
  <<2, 25>, <1, 1>>，这里的Key是<2, 25>，Value集合是<1, 1>。
  
  
* reduce函数的输入输出

  - 输入：shuffle的输出 <<2, 25>, <1, 1>>，
  
  - Value集合里所有的数字被相加，然后输出。所以reduce的输出就是<<2, 25>, 2>。
  
![](https://static001.geekbang.org/resource/image/bc/57/bc088edf00478c835003272696c44c57.jpg)


**Hive就是自动将SQL生成了MapReduce代码**


#### Hive的架构


![](https://static001.geekbang.org/resource/image/26/ea/26287cac9a9cfa3874a680fdbcd795ea.jpg)


* DDL

我们通过Hive的Client（Hive的命令行工具，JDBC等）向Hive提交SQL命令。如果是创建数据表的DDL（数据定义语言），Hive就会通过执行引擎Driver将数据表的信息记录在Metastore元数据组件中，这个组件通常用一个关系数据库实现，记录表名、字段名、字段类型、关联HDFS文件路径等这些数据库的Meta信息（元信息）。


* DQL

如果我们提交的是查询分析数据的DQL（数据查询语句），Driver就会将该语句提交给自己的编译器Compiler进行语法分析、语法解析、语法优化等一系列操作，最后生成一个MapReduce执行计划。然后根据执行计划生成一个MapReduce的作业，提交给Hadoop MapReduce计算框架处理。


Hive内部预置了很多函数，Hive的执行计划就是根据SQL语句生成这些函数的DAG（有向无环图），然后封装进MapReduce的map和reduce函数中


## Spark


**Why Spark?**

Spark和MapReduce相比：


   * 有更快的执行速度,甚至可以比 MapReduce 快100倍，

   * Spark和MapReduce相比，编程模式更简单


使用scala实现word_count,只需要3行

```
val textFile = sc.textFile("hdfs://...")
val counts = textFile.flatMap(line => line.split(" "))
                 .map(word => (word, 1))
                 .reduceByKey(_ + _)
counts.saveAsTextFile("hdfs://...")
```


### Spark架构核心元素的RDD


RDD是Spark的核心概念，是弹性数据集（Resilient Distributed Datasets）的缩写。


RDD既是Spark面向开发者的编程模型，又是Spark自身架构的核心元素。

* **RDD作为编程模型**

    Spark与MapReduce不同，则直接针对数据进行编程，将大规模数据集合抽象成一个RDD对象，然后在这个RDD上进行各种计算处理，得到一个新的RDD，
    继续计算处理，直到得到最后的结果数据。所以Spark可以理解成是面向对象的大数据计算。


    RDD上定义的函数分两种：

    * 转换（transformation）函数，这种函数的返回值还是RDD；

    * 执行（action）函数，这种函数不再返回RDD。


    RDD定义了很多转换操作函数，比如有计算map(func)、过滤filter(func)、合并数据集union(otherDataset)、根据Key聚合reduceByKey(func, [numPartitions])、连接数据集join(otherDataset, [numPartitions])、分组groupByKey([numPartitions])等十几个函数。


* **RDD作为核心架构**


    跟MapReduce一样，Spark也是对大数据进行分片计算，Spark分布式计算的**数据分片、任务调度都是以RDD为单位展开的**，每个RDD分片都会分配到一个执行进程去处理。


    RDD上的转换操作又分为两种：


     * RDD不会产生新分片的操作： 比如map、filter等，也就是说一个RDD数据分片，经过map或者filter转换操作后，结果还在当前分片


     * RDD会产生新的分片：比如reduceByKey，来自不同分片的相同Key必须聚合在一起进行操作，这样就会产生新的RDD分片


**spark的惰性计算**

'rdd2 = rdd1.map(func)'

并不会产生真正的新的物理RDD分片，物理上，Spark只有在产生新的RDD分片时候，才会真的生成一个RDD，Spark的这种特性也被称作惰性计算。


实际上，在理解Spark的过程中，应用程序代码中的RDD和Spark执行过程中生成的物理RDD不是一一对应的，需要进行区分。



### Spark的架构原理


**DAG**


首先谈起DAG我们先了解一下**计算阶段**的概念：

  Spark可以根据应用的复杂程度，分割成更多的计算阶段（stage），这些计算阶段组成一个有向无环图DAG，Spark任务调度器可以根据DAG的依赖关系执行计算阶段。


所谓DAG也就是有向无环图，满足以下条件：

 * 不同阶段的依赖关系是有向的，

 * 计算过程只能沿着依赖关系方向执行，被依赖的阶段执行完成之前，依赖的阶段不能开始执行，

 * 这个依赖关系不能有环形依赖，否则就成为死循环了。
 
 
 下面这个例子🌰描述了一个典型的Spark运行DAG的不同阶段。


![](https://static001.geekbang.org/resource/image/c8/db/c8cf515c664b478e51058565e0d4a8db.png)


从图上看，整个应用被切分成3个阶段，阶段3需要依赖阶段1和阶段2，阶段1和阶段2互不依赖。Spark在执行调度的时候，先执行阶段1和阶段2，完成以后，再执行阶段3。


{:.box-warning}

所以，你可以看到Spark作业调度执行的核心是DAG，有了DAG，整个应用就被切分成哪些阶段，每个阶段的依赖关系也就清楚了。

之后再根据每个阶段要处理的数据量生成相应的任务集合（TaskSet），每个任务都分配一个任务进程去处理，Spark就实现了大数据的分布式计算。


具体来看的话，负责Spark应用DAG生成和管理的组件是DAGScheduler，DAGScheduler根据程序代码生成DAG，然后将程序分发到分布式计算集群，按计算阶段的先后关系调度执行。



**Spark划分计算阶段的依据是什么呢？**


计算阶段划分的依据是shuffle，Spark的DAGScheduler在遇到shuffle的时候，会生成一个计算阶段。不是转换函数的类型，有的函数有时候有shuffle，有时候没有。


比如上图例子中RDD B和RDD F进行join，得到RDD G，这里的RDD F需要进行shuffle，RDD B就不需要。

因为RDD B在前面一个阶段，阶段1的shuffle过程中，已经进行了数据分区。分区数目和分区Key不变，就不需要再进行shuffle


**宽窄依赖**


 * 宽依赖：需要进行shuffle的依赖，被称作宽依赖

 * 窄依赖：这种不需要进行shuffle的依赖，在Spark里被称作窄依赖
 
 
跟MapReduce一样，shuffle也是Spark最重要的一个环节，只有通过shuffle，相关数据才能互相计算，构建起复杂的应用逻辑。


#### MapReduce 和 Spark的内在联系

梳理了shuffle在MapReduce中和在Spark中的应用（划分计算阶段），我们发现MapReduce和Spark存在一种内在联系。

其实Saprk可以当作MapReduce计算模型的一种不同实现：


 * Hadoop MapReduce：简单粗暴地根据shuffle将大数据计算分成Map和Reduce两个阶段。
 
 * Spark：更细腻一点，将前一个的Reduce和后一个的Map连接起来，当作一个阶段持续计算，形成一个更加优雅、高效地计算模型，虽然其本质依然是Map和Reduce。但是这种多个计算阶段依赖执行的方案可以有效减少对HDFS的访问，减少作业的调度执行次数，因此执行速度也更快


并且和Hadoop MapReduce主要使用磁盘存储shuffle过程中的数据不同，**Spark优先使用内存进行数据存储**，包括RDD数据。
除非是内存不够用了，否则是尽可能使用内存， 这也是Spark性能比Hadoop高的另一个原因。


**作业、计算任务和任务的时间先后关系**


* 作业（job）：DAGSchedualr在遇到action函数（不返回RDD的函数，如count()）的时候，会生成一个作业

* 计算任务（task）：RDD里面的每个数据分片，Spark都会创建一个计算任务去处理，所以一个计算阶段会包含很多个计算任务。


关于作业、计算阶段、任务的依赖和时间先后关系你可以通过下图看到。

![](https://static001.geekbang.org/resource/image/2b/d0/2bf9e431bbd543165588a111513567d0.png)


* 图中横轴方向是时间，纵轴方向是任务。两条粗黑线之间是一个作业，两条细线之间是一个计算阶段。

* 一个作业至少包含一个计算阶段。水平方向红色的线是任务，每个阶段由很多个任务组成，这些任务组成一个任务集合。


DAGScheduler根据代码生成DAG图以后，Spark的任务调度就以任务为单位进行分配，将任务分配到分布式集群的不同机器上执行。



#### Spark的架构


Spark的模块

* drive 驱动程序：Application 的驱动程序。可以理解为使程序运行中的 main 函数

* cluster manager：Spark 的集群管理器，主要负责对整个集群资源的分配和管理。

* workers：Spark 的工作节点，用于执行提交的作业

* executor：真正执行计算任务的组件。

* task：Spark给执行单元发的最小工作单位


![](https://static001.geekbang.org/resource/image/16/db/164e9460133d7744d0315a876e7b6fdb.png)


上面这张图是Spark的运行流程，我们一步一步来看。


```
1.首先，Spark应用程序启动在自己的JVM进程里，即Driver进程，启动后调用SparkContext初始化执行配置和输入数据。
SparkContext启动DAGScheduler构造执行的DAG图，切分成最小的执行单位也就是计算任务。


2.然后Driver向Cluster Manager请求计算资源，用于DAG的分布式计算。
Cluster Manager收到请求以后，将Driver的主机地址等信息通知给集群的所有计算节点Worker。


3.Worker收到信息以后，根据Driver的主机地址，跟Driver通信并注册，然后根据自己的空闲资源向Driver通报自己可以领用的任务数。
Driver根据DAG图开始向注册的Worker分配任务。


4.Worker收到任务后，启动Executor进程开始执行任务。
Executor先检查自己是否有Driver的执行代码，如果没有，从Driver下载执行代码，通过Java反射加载后开始执行。
```

总结来说，Spark有三个主要特性：**RDD的编程模型更简单，DAG切分的多阶段计算过程更快速，使用内存存储中间计算结果更高效**。
这三个特性使得Spark相对Hadoop MapReduce可以有更快的执行速度，以及更简单的编程实现。



## HBase


之前数据库一直是关系数据库的天下，但是关系数据库具有糟糕的海量数据处理能力和僵硬的设计约束。

NoSQL的概念被提出，NoSQL，主要指非关系的、分布式的、支持海量数据存储的数据库设计模式。简单的说，NoSQL数据库则简单暴力地认为，数据库就是存储数据的，业务逻辑应该由应用程序去处理。



#### HBase的框架


我们先来看一看HBase的架构设计，HBase为可伸缩海量数据储存而设计，实现面向在线业务的实时数据访问延迟。

HBase的伸缩性主要依赖其可分裂的HRegion及可伸缩的分布式文件系统HDFS实现


![](https://static001.geekbang.org/resource/image/9f/f7/9f4220274ef0a6bcf253e8d012a6d4f7.png)


* HRegion是HBase负责数据存储的主要进程，应用程序对数据的读写操作都是通过和HRegion通信完成.

 在HBase中，数据以HRegion为单位进行管理，也就是说应用程序如果想要访问一个数据，必须先找到HRegion，然后将数据读写操作提交给HRegion，由 HRegion完成存储层面的数据操作。


* HRegionServer是物理服务器，每个HRegionServer上可以启动多个HRegion实例。
 
 -当一个 HRegion中写入的数据太多，达到配置的阈值时，一个HRegion会分裂成两个HRegion，并将HRegion在整个集群中进行迁移，以使HRegionServer的负载均衡。


* HMaster：每个HRegion中存储一段Key值区间\[key1, key2)的数据，所有HRegion的信息，包括存储的Key值区间、所在HRegionServer地址、访问端口号等，都记录在HMaster服务器上。

  -为了保证HMaster的高可用，HBase会启动多个HMaster，并通过ZooKeeper选举出一个主服务器。


**数据读过程**

应用程序通过ZooKeeper获得主HMaster的地址，输入Key值获得这个Key所在的HRegionServer地址，然后请求HRegionServer上的HRegion，获得所需要的数据。


**数据写入过程**

需要先得到HRegion才能继续操作。HRegion会把数据存储在若干个HFile格式的文件中，这些文件使用HDFS分布式文件系统存储，在整个集群内分布并高可用。

 -当一个HRegion中数据量太多时，这个HRegion连同HFile会分裂成两个HRegion，并根据集群中服务器负载进行迁移。

 -如果集群中有新加入的服务器，也就是说有了新的HRegionServer，由于其负载较低，也会把HRegion迁移过去并记录到HMaster，从而实现HBase的线性伸缩。



#### HBase数据结构上的优化


HBase支持列族结构的NoSQL数据库，在创建表的时候，只需要指定列族的名字，无需指定字段（Column）。那什么时候指定字段呢？可以在数据写入时再指定。

通过row key来找到数据，没有schema，不需要大量的数据冗余。

一个列中的类型可以是不一样的

可以找到之前的版本的数据

列中的data可以不存在


#### HBase数据存储的高可用性


传统的机械式磁盘的访问特性是连续读写很快，随机读写很慢。为了提高数据写入速度，HBase使用了一种叫作LSM树的数据结构进行数据存储。


* LSM树的全名是Log Structed Merge Tree，翻译过来就是Log结构合并树。数据写入的时候以Log方式连续写入，然后异步对磁盘上的多个LSM树进行合并。


LSM树可以看作是一个N阶合并树。

 -数据写操作（包括插入、修改、删除）都在内存中进行，并且都会创建一个新记录（修改会记录新的数据值，而删除会记录一个删除标志）。

 -这些数据在内存中仍然还是一棵排序树，当数据量超过设定的内存阈值后，会将这棵排序树和磁盘上最新的排序树合并。
 
 -当这棵排序树的数据量也超过设定阈值后，会和磁盘上下一级的排序树合并。合并过程中，会用最新更新的数据覆盖旧的数据（或者记录为不同版本）


在LSM树上进行一次数据更新不需要磁盘访问，在内存即可完成。当数据访问以写操作为主，而读操作则集中在最近写入的数据上时，使用LSM树可以极大程度地减少磁盘的访问次数，加快访问速度


**小结：**


架构上通过数据分片的设计配合HDFS，实现了数据的分布式海量存储；数据结构上通过列族的设计，实现了数据表结构可以在运行期自定义；存储上通过LSM树的方式，使数据可以通过连续写磁盘的方式保存数据，极大地提高了数据写入性能。


**列族数据库的缺点**


1:列族不好查询，没有传统sql那样按照不同字段方便，只能根据rowkey查询，范围查询scan性能低

2:查询也没有mysql一样的走索引优化，因为列不固定 

3:列族因为不固定，所以很难做一些业务约束，



## 流计算


**Spark Streaming**


Spark Streaming巧妙地利用了Spark的分片和快速计算的特性，将实时传输进来的数据按照时间进行分段，把一段时间传输进来的数据合并在一起，当作一批数据，再去交给Spark去处理。下图这张图描述了Spark Streaming将数据分段、分批的过程。

![](https://static001.geekbang.org/resource/image/fb/c3/fb535e9dc1813dbacfa03c7cb65d17c3.png)

如果时间段分得足够小，每一段的数据量就会比较小，再加上Spark引擎的处理速度又足够快，这样看起来好像数据是被实时处理的一样，这就是Spark Streaming实时流计算的奥妙。


## ZooKeeper


HDFS和HBase架构分析时都提到了ZooKeeper,我们比较常用的多台服务器状态一致性的解决方案就是ZooKeeper。


**Paxos算法和ZooKeeper**

```
Paxos算法就是用来解决这类问题的，多台服务器通过内部的投票表决机制决定一个数据的更新与写入。

应用程序连接到任意一台服务器后提起状态修改请求,

会将这个请求发送给集群中其他服务器进行表决。

如果某个服务器同时收到了另一个应用程序同样的修改请求，它可能会拒绝服务器1的表决，并且自己也发起一个同样的表决请求，那么其他服务器就会根据时间戳和服务器排序规则进行表决。

表决结果会发送给其他所有服务器，最终发起表决的服务器也就是服务器1，会根据收到的表决结果决定该修改请求是否可以执行，从而在收到请求的时候就保证了数据的一致性。

```

Paxos算法比较复杂，为了简化实现，ZooKeeper使用了一种叫ZAB（ZooKeeper Atomic Broadcast，ZooKeeper原子消息广播协议）的算法协议。

有三种状态信息：

 -Looking ：选举状态。

 -Following ：Follower节点（从节点）所处的状态。

 -Leading ：Leader节点（主节点）所处状态。

* 最大ZXID的概念：节点本地的最新事务编号，包含epoch和计数两部分。


ZAB的恢复过程如下，三个阶段：


1. 选举阶段

  -此时集群中的节点处于Looking状态。它们会各自向其他节点发起投票，投票当中包含自己的服务器ID和最新事务ID（ZXID）。
  
  -接下来，节点会用自身的ZXID和从其他节点接收到的ZXID做比较，如果发现别人家的ZXID比自己大，也就是数据比自己新，那么就重新发起投票，投票给目前已知最大的ZXID所属节点。
  
  -每次投票都会做票数统计，如果有投票超过半数，那么这个结点就成为准leader，状态为Leading，其他结点为Following


2. 发现阶段：用于在从节点中发现最新的ZXID和事务日志。
  
  -Leader集思广益，接收所有Follower发来各自的最新epoch值。Leader从中选出最大的epoch，基于此值加1，生成新的epoch分发给各个Follower。
  
  -各个Follower收到全新的epoch后，返回ACK给Leader，带上各自最大的ZXID和历史事务日志。Leader选出最大的ZXID，并更新自身历史日志。


3. 同步阶段

  -把Leader刚才收集得到的最新历史事务日志，同步给集群中所有的Follower。只有当半数Follower同步成功，这个准Leader才能成为正式的Leader。
  
  

基于ZAB算法，ZooKeeper集群保证数据更新的一致性，并通过集群方式保证ZooKeeper系统高可用。




但是ZooKeeper系统中所有服务器都存储相同的数据，也就是数据没有分片存储，因此不满足分区耐受性。


ZooKeeper通过一种树状结构记录数据，如下图所示。


![](https://static001.geekbang.org/resource/image/76/5f/76526be77b0026a0c3b2d661d362665f.png)


应用程序可以通过路径的方式访问ZooKeeper中的数据，比如/services/YaView/services/stupidname这样的路径方式修改、读取数据。


每个树上的结点称为 _Znode_ ,Znode包含了数据、子结点引用、权限信息等。一个Znode可用如下表示

![](https://user-gold-cdn.xitu.io/2018/5/22/16385a1ecf740084?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

每个节点的数据最大不能超过1MB。


ZooKeeper还支持监听模式，当数据发生改变的时候，通知应用程序。


因为大数据系统通常都是主从架构，为了保证集群状态一致防止“脑裂”，所以运行期只能有一个主服务器工作（active master），但是为了保证高可用，必须有另一个standby master。


那么应用程序和集群其他服务器如何才能知道当前哪个服务器是实际工作的主服务器呢？


一台主服务器启动后向ZooKeeper注册自己为当前工作的主服务器，因此另一台服务器就只能注册为热备主服务器，应用程序运行期都和当前工作的主服务器通信。

如果当前工作的主服务器宕机（在ZooKeeper上记录的心跳数据不再更新），热备主服务器通过ZooKeeper的监控机制发现当前工作的主服务器宕机，就向ZooKeeper注册自己成为当前工作的主服务器。应用程序和集群其他服务器跟新的主服务器通信，保证系统正常运行。

因为ZooKeeper系统的多台服务器存储相同数据，并且每次数据更新都要所有服务器投票表决，所以和一般的分布式系统相反，ZooKeeper集群的性能会随着服务器数量的增加而下降。




## Kafka


**什么是Kafka**：kafka是一个消息队列，把消息放到队列里面的是生产者，从消息队列里面取消息的是消费者


**Kafka的分布式**

消息队列可以有多个，每个消息队列有自己的topic。一个topic可以有多个生产者和多个消费者。

为了保证一个队列的高吞吐量，Kafka对topic进行分区。

一个Kafka服务器叫做broker，kafka集群包含了多个Kafka服务器。

一个topic分为多个partition,每个partition分布在不同的broker中，Kafka形成了一个天然的分布式


**Kafka的高可用**

那么问题来了，消息放在不同的分区，存在不同的broker中，万一一个broker挂了怎么办？

因此，Kafka会对每个broker的分区做备份。在一个broker中有一个主分区，和几个备份分区。

  - 生产者消费者与主分区做交互。
  
  - 备份分区仅做备份，不读写
  
 当一个分区挂掉了，就会选取其他broker的分区作为主分区


**Kafka的持久化**

分区怎么实现持久化呢？

Kafka将分区的数据写在磁盘中的消息日志中，只允许追加填写，避免了随机访问。

Kafka并不是一有消息立即写日志，它会先写入缓存，等到数量足够多再写入磁盘。


**Kafka的消费者**

Kafka将多个消费者组成一个消费者组，组中每个消费者都消费一个不同的分区，可以提高吞吐量

- 如果某个消费者挂了，那么其他消费者自动补全

- 消费者多于分区的话，会有空闲的消费者

- 消费者组直接是独立的


消费者怎么知道自己读到哪里了？

每个消费者有自己的offset来表示消费进度，每次消费者消费的时候都提交这个offset

Kafka读数据的巧妙：

正常的读数据都是将内核态数据拷贝到用户态的，但是kafka sendfile()直接在内核态进行拷贝，少了一步操作。


