---
title: MVCC(多版本并发控制)
tags: [事务]
categories: 
    - [学习,事务,MVCC多版本并发控制]
---
[数据库事务、事务隔离级别以及锁机制详解](https://www.cnblogs.com/jieerma666/p/10805578.html)
[【眼见为实】自己动手实践理解数据库REPEATABLE READ && Next-Key Lock](https://www.cnblogs.com/songwenjie/p/8643684.html)
[白话数据库中的MVCC](https://cloud.tencent.com/developer/article/1357157)
[正确的理解MySQL的MVCC及实现原理](https://blog.csdn.net/SnailMann/article/details/94724197)
# 什么是MVCC
首先需要说明的是MVCC是一种概念原理，由于并发加锁会造成性能问题，所以出现这种乐观锁的方式，不使用锁的非阻塞并发读来提高性能的前提下，来解决不可重复读以及幻读的问题
# 什么是当前读和快照读
想要明白MVCC就需要先了解什么是当前读和快照读
- 当前读：（悲观锁实现）像`select ... lock in share mode`(共享锁)，`select ... for update`，`update`，`insert`，`delete`(排他锁)这些`加锁`的操作就都是一种当前读，为什么叫当前读？就是它读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。
- 快照读：（乐观锁实现）像`不加锁`的select操作就是快照读，即不加锁的非阻塞读，前提是隔离级别非串行，不然会退化为当前读，快照读的实现就是基于多版本并发控制，即`MVCC`，它是行锁的一个变种，避免加锁，降低开销，既然是基于多版本，即快照读获得的数据并不一定是最新的版本，而可能是历史版本。
# MVCC，快照读的关系

# 核心思路
通过设置一种递增的事务ID或版本号ID，每次事务执行ID就会+1,假设每条数据后面都隐藏两个字段（新增版本）