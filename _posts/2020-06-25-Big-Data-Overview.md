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


- ### MapReduce


MapReduce既是一个编程模型又是一个计算框架


![](https://static001.geekbang.org/resource/image/55/ba/5571ed29c5c2254520052adceadf9cba.png)


MapReduce编程模型：包含 Map 和 Reduce 两个过程，Map阶段给每个数据块分配一个map，key相同的数据分配给同一个Reduce，Reduce阶段进行count处理


#### MapReduce计算框架是如何运作的


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


##### shufflr机制


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
  
  
* **WHAT**

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
    
 **Yarn**
  
  

### Hadoop 1.0 和 Hadoop2.0 的区别：


* Hadoop1：我收到任务，由我来拆解任务成子任务，并且联系下方的同学谁有空就谁做，有任务的同学单独向我汇报进度。

* Hadoop2:我知道有个任务要处理，在同学中选中一位班长，由班长来拆解子任务并告诉我需要的多少同学去做，由班长收集进度并汇报给我。

这2种方式，同学们之间都系会有内容传递，把“移动的不是数据而是计算”改成“尽量少移动数据，分配计算任务”是否更合适？而如何合理分配计算是不是更值得探讨？

