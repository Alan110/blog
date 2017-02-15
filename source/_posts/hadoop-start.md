---
title: hadoop入门解惑
date: 2017-02-15 07:36:59
tags:
---

##  Hadoop

Hadoop是一个由Apache基金会所开发的分布式系统基础架构,用户可以在不了解分布式底层细节的情况下，开发分布式程序。充分利用集群的威力进行高速运算和存储。

Hadoop的框架最核心的设计就是：HDFS和MapReduce。HDFS为海量的数据提供了存储，则MapReduce为海量的数据提供了计算。

可以把它想象成一个云端的服务接口。


## 官方介绍

Hadoop MapReduce is a software framework for easily writing applications which process vast amounts of data (multi-terabyte data-sets) in-parallel on large clusters (thousands of nodes) of commodity hardware in a reliable, fault-tolerant manner.

A MapReduce job usually splits the input data-set into independent chunks which are processed by the map tasks in a completely parallel manner. The framework sorts the outputs of the maps, which are then input to the reduce tasks. Typically both the input and the output of the job are stored in a file-system. The framework takes care of scheduling tasks, monitoring them and re-executes the failed tasks.

Typically the compute nodes and the storage nodes are the same, that is, the MapReduce framework and the Hadoop Distributed File System (see HDFS Architecture Guide) are running on the same set of nodes. This configuration allows the framework to effectively schedule tasks on the nodes where data is already present, resulting in very high aggregate bandwidth across the cluster.

The MapReduce framework consists of a single master JobTracker and one slave TaskTracker per cluster-node. The master is responsible for scheduling the jobs' component tasks on the slaves, monitoring them and re-executing the failed tasks. The slaves execute the tasks as directed by the master.

Minimally, applications specify the input/output locations and supply map and reduce functions via implementations of appropriate interfaces and/or abstract-classes. These, and other job parameters, comprise the job configuration. The Hadoop job client then submits the job (jar/executable etc.) and configuration to the JobTracker which then assumes the responsibility of distributing the software/configuration to the slaves, scheduling tasks and monitoring them, providing status and diagnostic information to the job-client.

Although the Hadoop framework is implemented in JavaTM, MapReduce applications need not be written in Java.

Hadoop Streaming is a utility which allows users to create and run jobs with any executables (e.g. shell utilities) as the mapper and/or the reducer.
Hadoop Pipes is a SWIG- compatible C++ API to implement MapReduce applications (non JNITM based).

-------------

hadoop MapReduce 是一个为了更简单的编写，大数据集，大量节点并行计算应用的软件框架，它使得硬件可靠，高容错性。

一个 MapReduce job 通常 将输入数据集划分成多个独立的数据块，便于map任务的并行计算，管理。MapReduce对map的结果进行排序，传递给reduce任务。通常，输入和输出都存在一个文件系统。Mapreduce 关心人物分配，监控和重执行失败的任务。

通常，计算节点和存储节点是同一个地方，这就是MapReduce框架和Hadoop的分布式文件系统HDFS，跑在了这些节点上。通过配置，可以高效的任务调度当前数据存在的节点，跨节点的高带宽传输计算结果。

**MapReduce框架由一个JobTracker 和每个计算节点上的TaskTracker(奴隶机)** 组成。JobTracker负责调度job的分片任务到相关的奴隶机上。监控他们，并且重新执行失败的任务。奴隶机执行任务，就好像由master执行的一样。

**简单来说，MapReduce 应用指定输入输出路径，以及通过使用合适的接口或者抽象类实现map 和reduce方法。**job有很多参数，形成了job的配置。Hadoop job 客户端提交job。使得JobTracker分发程序和配置到奴隶机上，并且任务调度，监控他们。同时提供状态信息给job客户端

**虽然Hadoop是用java写的，但是MapReduce程序可以用任意语言来实现。**
Hadoop提供了2种额外的接口
Hadoop Streaming   unix标准输入输出(只支持文本)
Hadoop Pipes  c++接口


## HDFS

分布式文件系统, 可以把它想象成一个云端的公用硬盘, 存放我们的输入，输出和中间计算结果。


## MapReduce

是一个经典的编程模型,利用分而治之的思想
map是分，reduce是合的概念。将整体抽象为相同行为的个体分别计算，然后将他们合并。

只需要一个数据单元就可以进行的运算在Map方法中实现。
需要多个数据单元才能进行的运算在Reduce方法中实现。


## hadoop String


## 简单例子
