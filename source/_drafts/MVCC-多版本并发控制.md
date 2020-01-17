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

# MVCC，快照读，当前读的关系
- 准确来讲，MVCC多版本控制指的是`维持一个数据的多个版本，使得读写操作没有冲突`这么一个概念，仅仅是一个理想概念
- 而在MySQL中，实现这个MVCC理想概念就需要提供具体功能实现它，`快照读就是其实现MVCC理想模型的其中一个具体非阻塞读功能`，当前读是悲观锁的具体功能实现
- 更具体来说，快照读和当前读本身也是一个抽象概念，深入来说，当前读实现方式就是上面的那些加锁操作，而快照读深入实现方式则是`由3个隐式字段，undo日志，ReadView完成的`

# MVCC概念的核心思路
通过设置一种递增的事务ID或版本号ID，每次事务执行ID就会+1,假设每条数据后面都隐藏两个字段（新增版本I，删除版本D），当前事务版本为C
> SELECT:
    读取`(I<=C) && (D==null || D>=C)`的记录
  INSERT:
    `新插入一行，将C保存至行的I`的字段
  UPDATE:
    `新插入一行，将C保存至行的I`的字段，同时将`原记录行的D设置为C`
  DELETE:
    将`C保存至行的D`的字段

例如：
books表中有一条数据，版本号为1：

|表数据|新增版本(I)|删除版本(D)|
|-|-|-|
|books1|1||

事务A，版本号为2：select * from books；`(I<=2) && (D==null || D>=2)`，可以读取到books1
事务B，版本号为3：insert into books...；插入后事务结束：

|表数据|新增版本(I)|删除版本(D)|
|-|-|-|
|books1|1||
|books2|3||

事务A，版本号为2：select * from books；`(I<=2) && (D==null || D>=2)`，还是只可以读取到books1，解决了幻读

# MySQL中具体实现MVCC的原理
MySQL中为了解决读写冲突问题，采用了MVCC概念，具体实现原理依赖记录中的`3个隐式字段，undo日志，ReadView`来实现的

## 隐式字段
数据库中每行记录除了我们自己自定义的字段外，还有隐式定义的`DB_TRX_ID,DB_ROLL_PTR,DB_ROW_ID`等字段
- `DB_TRX_ID`:
    6byte，最新最近事务ID；记录创建/最后一次修改该记录的事务ID
- `DB_ROLL_PTR`:
    7byte，回滚指针，指向这条记录的上一个版本地址（存储于`rollback segment`里）
- `DB_ROW_ID`:
    6byte，隐含的自增ID（隐藏主键），如果表没有主键，InnoDB会自动以`DB_ROW_ID`产生一个聚簇索引
  实际还有一个删除flag隐藏字段`deleted_bit`，既记录被更新或删除并不代表真的删除，而是删除flag变了
![表内容](MVCC-多版本并发控制/MVCC1.png)如上图，`DB_ROW_ID`是数据库默认为该记录生成的唯一隐式主键，`DB_TRX_ID`是该记录的最终最新的事务ID，`DB_ROLL_PTR`是一个回滚指针，用于配合undo日志，指向上一个旧版本

## undo日志
undo log主要分为两种：
- insert undo log
    事务在`insert`新纪录时产生的`undo log`，只在事务回滚时需要，并且在事务提交后可以被立即丢弃
- update undo log
    事务在`update`或`delete`时产生的`undo log`，不仅在事务回滚时需要，在快照读时也需要；
    所以不能随便删除，只有在快照读和事务回滚不涉及该日志时，对应的日志才会被`purge`线程统一清除
> purge
>  - 从前面的分析可以看出，为了实现InnoDB的MVCC机制，更新或者删除操作都只是设置一下老记录的deleted_bit，并不真正将过时的记录删除。
>  - 为了节省磁盘空间，InnoDB有专门的purge线程来清理deleted_bit为true的记录。为了不影响MVCC的正常工作，purge线程自己也维护了一个read view（这个read view相当于系统中最老活跃事务的read view）;如果某个记录的deleted_bit为true，并且DB_TRX_ID相对于purge线程的read view可见，那么这条记录一定是可以被安全清除的。
> <font color="red" face="黑体">也就是说当某条记录的`deleted_bit`为true，已经删除了；并且其`DB_TRX_ID`对于purge的read view这个最老的视图都认为它可见，确定操作过了的，一定就是可以被安全删除的。</font>

对MVCC有帮助的实质是`update undo log`，`undo log`实际上就是存在`rollback segment`中的旧记录链
