---
title: Mysql 二
date: 2023-10-03 00:29:46
---


### 触发器


## 锁

![[Mysql锁_图1.png]]

![[Mysql锁_图2.png]]


### 全局锁

![[Mysql锁_全局锁_图1.png]]

![[Mysql锁_全局锁_图2.png]]

![[Mysql锁_全局锁_图3.png]]

### 表级锁

#### 表锁

表共享读锁（read lock 简称读锁）

当一个会话中使用了读锁 （lock tables 表名 read）， 那么当前的会话/客户端 和其他的会话/客户端 都可以读取该表；但是如果当前会话去写入的话，会抛出异常，其他的会话/客户端 写表的话（DDL/DML）会出现阻塞。只有当前会话/客户端 讲锁释放，其他会话/客户端 才会进行下去


表独占写锁 （write lock 简称解锁）

当一个会话中使用了写锁 （lock tables 表名 write），那么不会阻塞或阻止当前会话的任何操作，但是其他会话/客户端既不能读也不能写

![[Mysql锁_表锁_图1.png]]


#### 元数据锁

在表上有活动事务时，不能修改表结构
![[Mysql锁_元数据锁_图2.png]]

比如 两个事务开启时，可以同时对表进行读写操作，是因为SHARED_READ 和 SHARED_WRITE 相互兼容。但是 如果此时有个事务 用了 alter table 这种语句，那么这个事务会被阻塞，因为alter table会默认使用X锁，是排他锁。与SHARED_READ 和 SHARED_WRITE 锁互斥。

使用 
``` sql

select object_type, object_schema, object_name, lock_type, lock_duration from performance_schema.metadata_locks;

-# performance_schema 是系统表

```

就可以查看当前数据库表当中的元数据锁

![[Mysql锁_元数据锁_图3.png]]



#### 意向锁

##### 意向共享锁（IS）：与读锁（表锁共享锁）兼容，与表锁排他锁（写锁） 互斥



##### 意向排他锁： 与读锁和写锁 都互斥，意向锁之间不会互斥。


![[Mysql锁_意向锁_图1.png]]


在 用到增删改查命令时，也会用到行锁，如上图所示，在此时要用到表锁的话，会对A线程每一行数据进行查看是否有添加行锁。每行都加行锁很带来负担，因此MySQL用到了意向锁来改进，并且查看线程A用到的锁是否和线程B相加的锁时兼容的。

``` bsh

mysql> select object_schema, object_name, index_name, lock_type, lock_mode, lock_data from performance_schema.data_locks\G;

*************************** 1. row ***************************
object_schema: test
  object_name: t1
   index_name: NULL
    lock_type: TABLE
    lock_mode: IS
    lock_data: NULL
*************************** 2. row ***************************
object_schema: test
  object_name: t1
   index_name: GEN_CLUST_INDEX
    lock_type: RECORD
    lock_mode: S
    lock_data: supremum pseudo-record
*************************** 3. row ***************************
object_schema: test
  object_name: t1
   index_name: GEN_CLUST_INDEX
    lock_type: RECORD
    lock_mode: S
    lock_data: 0x000000000200
*************************** 4. row ***************************
object_schema: test
  object_name: t1
   index_name: GEN_CLUST_INDEX
    lock_type: RECORD
    lock_mode: S
    lock_data: 0x000000000201
*************************** 5. row ***************************
object_schema: test
  object_name: t1
   index_name: GEN_CLUST_INDEX
    lock_type: RECORD
    lock_mode: S
    lock_data: 0x000000000202
5 rows in set (0.01 sec)

```

RECORD就代表行锁

### 行级锁
行级锁 每次操作锁住对应的行数据。锁定颗粒度最小，发生锁冲突的概率最低，并发度最高。

![[Mysql锁_行级锁_图1.png]]


#### 间隙锁/ 临键锁

![[Mysql锁_临键锁_图1.png]]

![[Mysql锁_临键锁_图2.png]]

![[Mysql锁_临键锁_图3.png]]

https://blog.csdn.net/aliyunyyds/article/details/140539147

https://zhuanlan.zhihu.com/p/579701060

## Innodb

![[Innodb_架构_图1.png]]

![[Innodb_架构_图2.png]]

![[Innodb_架构_图3.png]]
![[Innodb_架构_图4.png]]

4
![[Innodb_架构_图5.png]]

![[Mysql_Innodb_后台线程_图1.png]]

### 事务原理
![[Mysql_Innodb_事务原理_图1.png]]


#### MVCC (多版本并发控制)

指维护一个数据的多个版本，使得读写操作没有冲突，快照读为Mysql实现MVCC 提供了一个非阻塞读功能。 MVCC具体实现，还需要依赖于数据库记录中的三个隐式字段，undo log 日志，readView
![[Mysql_Innodb_MVCC_图1.png]]

##### 隐藏字段

DB_TRX_ID
DB_ROLL_PTR 回滚指针
DB_ROW_ID 表中无主键 才会用到

![[Mysql_Innodb_MVCC_图2.png]]

mysql数据存放在 /var/lib/mysql
指令： idb2sdi 查看idb文件中字典信息