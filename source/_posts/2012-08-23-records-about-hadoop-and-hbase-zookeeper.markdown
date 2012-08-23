---
layout: post
title: "一些关于HBase及Zookeeper的运维记录"
date: 2012-08-23 23:01
comments: true
categories: [Hadoop,Hbase,应用运维]
---
1、关于Zookeeper日志记录及快照定期清理维护

HBase或Zookeeper运作一段时间长的话，日志及快照文件大小是相当占用磁盘空间，这是个需注意的地方，否则磁盘空间既浪费又易爆。而Zookeeper这些日志文件又不能随意轻易删除。对于需谨慎安全的定期做清理工作，之前Zookeeper早期版本据说就提供了一个PurgeTxnLog的小工具。

不过今天无意间在Zookeeper目录中bin目录下发现个zkCleanup.sh的可执行文件，经Google查询确认可更方便的用于合理安全的清理这些日志记录和快照文件。

就简单的一条命令即可搞定：
	/usr/yupoo/hadoop/zookeeper-3.4.3/bin/zkCleanup.sh <DataDir> <Count> 

备注：其中DataDir指的是配置文件中的日志文件及快照文件存放的路径地址；Count指的是要保留最近的数据份数。


