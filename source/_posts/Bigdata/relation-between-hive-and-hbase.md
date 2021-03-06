---
title: Hive和Hbase的关系
date: 2016-09-25 17:44:18
tags: [大数据, Hive, Hbase]
categories: 大数据
layout: post
updated:
comments:
permalink:
---

**导读**
- 什么是Hive？
- hive与hbase的关系

<!--more-->

# 什么是Hive？

Apache Hive数据仓库软件提供对存储在分布式中的大型数据集的查询和管理，它本身是建立在Apache Hadoop之上，主要提供以下功能：
1. 它提供了一系列的工具，可用来对数据进行提取/转化/加载（ETL）；
2. 是一种可以存储、查询和分析存储在HDFS（或者HBase）中的大规模数据的机制；
3. 查询是通过MapReduce来完成的（并不是所有的查询都需要MapReduce来完成，比如select * from XXX就不需要；
4. 在Hive0.11对类似select a,b from XXX的查询通过配置也可以不通过MapReduce来完成

</br>

上面的意思很明白了.这里再给他提炼一下：
1. hive是一个数据仓库。
2. hive基于hadoop。
总结为一句话：hive是基于hadoop的数据仓库。

</br>

**那么上面“基于”如何理解：**

Hive是一种建立在Hadoop文件系统上的数据仓库架构，并对存储在HDFS中的数据进行分析和管理。（也就是说对存储在HDFS中的数据进行分析和管理，我们不想使用手工，我们建立一个工具把，那么这个工具就可以是hive）

</br>

**那么，我们如何来分析和管理那些数据呢？**

Hive定义了一种类似SQL的查询语言，被称为HQL，对于熟悉SQL的用户可以直接利用Hive来查询数据。同时，这个语言也允许熟悉 MapReduce 开发者们开发自定义的mappers和reducers来处理内建的mappers和reducers无法完成的复杂的分析工作。Hive可以允许用户编写自己定义的函数UDF，来在查询中使用。Hive中有3种UDF：User Defined Functions（UDF）、User Defined Aggregation Functions（UDAF）、User Defined Table Generating Functions（UDTF）。

今天，Hive已经是一个成功的Apache项目，很多组织把它用作一个通用的、可伸缩的数据处理平台。

当然，Hive和传统的关系型数据库有很大的区别，Hive将外部的任务解析成一个MapReduce可执行计划，而启动MapReduce是一个高延迟的一件事，每次提交任务和执行任务都需要消耗很多时间，这也就决定Hive只能处理一些高延迟的应用（如果你想处理低延迟的应用，你可以去考虑一下Hbase）。同时，由于设计的目标不一样，Hive目前还不支持事务；不能对表数据进行修改（不能更新、删除、插入；只能通过文件追加数据、重新导入数据）；不能对列建立索引（但是Hive支持索引的建立，但是不能提高Hive的查询速度。如果你想提高Hive的查询速度，请学习Hive的分区、桶的应用）。

---------

# hive与hbase的联系与区别

## 共同点

hbase与hive都是架构在hadoop之上的。都是用hadoop作为底层存储

## 不同点

1. Hive是建立在Hadoop之上为了减少MapReduce jobs编写工作的批处理系统，HBase是为了支持弥补Hadoop对实时操作的缺陷的项目 。
2. 想象你在操作RMDB数据库，如果是全表扫描，就用Hive+Hadoop,如果是索引访问，就用HBase+Hadoop 。
3. Hive query就是MapReduce jobs可以从5分钟到数小时不止，HBase是非常高效的，肯定比Hive高效的多。
4. Hive本身不存储和计算数据，它完全依赖于HDFS和MapReduce，Hive中的表纯逻辑。
5. hive借用hadoop的MapReduce来完成一些hive中的命令的执行
6. hbase是物理表，不是逻辑表，提供一个超大的内存hash表，搜索引擎通过它来存储索引，方便查询操作。
7. hbase是列存储。
8. hdfs作为底层存储，hdfs是存放文件的系统，而Hbase负责组织文件。
9. hive需要用到hdfs存储文件，需要用到MapReduce计算框架。
