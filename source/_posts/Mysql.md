---
title: Mysql
date: 2023-09-26 01:21:46
---

# 基础

在数据库管理系统中，SQL（Structured Query Language）是一种用于访问和操作数据库的标准编程语言。SQL可以大致分为四个主要的部分，它们分别用于执行不同类型的数据库操作：DDL（数据定义语言）、DML（数据操纵语言）、DCL（数据控制语言）和DQL（数据查询语言）。下面是每个部分的简要说明：

1. **DDL（Data Definition Language）数据定义语言**：  
    DDL用于定义和修改数据库的结构。它允许用户创建、修改、删除数据库中的表、索引、视图等对象。常用的DDL语句包括：
    - `CREATE`：用于创建新的数据库对象，如表、索引等。
    - `ALTER`：用于修改现有数据库对象的结构，如添加、删除或修改表中的列。
    - `DROP`：用于删除数据库中的对象，如表、索引等。
    - `TRUNCATE`：用于删除表中的所有数据，但不删除表本身。
2. **DML（Data Manipulation Language）数据操纵语言**：  
    DML用于对数据库中的数据进行操作，如插入、更新、删除数据。它允许用户直接操作数据库中的数据。常用的DML语句包括：
    - `INSERT`：用于向表中插入新数据。
    - `UPDATE`：用于修改表中的数据。
    - `DELETE`：用于从表中删除数据。
3. **DCL（Data Control Language）数据控制语言**：  
    DCL用于定义数据库的访问权限和安全级别，控制数据的访问和修改。它主要用于设置用户权限、角色和权限的授予与回收等。常用的DCL语句包括：
    - `GRANT`：用于给用户或角色授予权限。
    - `REVOKE`：用于收回用户或角色的权限。
4. **DQL（Data Query Language）数据查询语言**：  
    DQL主要用于从数据库中检索数据，它使用户能够编写查询语句来查询表中的数据。DQL是SQL中最重要的部分之一，因为它允许用户以灵活的方式检索信息。最常用的DQL语句是：
    - `SELECT`：用于从数据库中查询数据。它可以单独使用，也可以与`FROM`、`WHERE`、`GROUP BY`、`HAVING`、`ORDER BY`等子句结合使用，以执行复杂的查询操作。

这四个部分共同构成了SQL语言的基础，允许用户执行各种数据库操作，从而有效地管理和利用存储在数据库中的数据。

```  sql
-- 修改表名
-- ALTER TABLE 旧表名 RENAME AS 新表名
ALTER TABLE teacher RENAME AS teachers;
 
-- 增加表的字段
-- ALTER TABLE 表名 ADD 字段名 列属性
ALTER TABLE teachers ADD age INT(11);
 
-- 修改表的字段(重命名，修改约束)
-- ALTER TABLE 表名 MODIFY 字段名 [列属性];
ALTER TABLE teachers MODIFY age VARCHAR(11);-- 修改约束
-- ALTER TABLE 表名 CHANGE 旧名字 新名字 [列属性];
ALTER TABLE teachers CHANGE age age1 INT(1);-- 字段重命名
 
-- 删除表的字段
-- ALTER TABLE 表名 DROP 字段名
ALTER TABLE teachers DROP age1;
```


``` sql

1. 操作数据库：CRUD
	1. C(Create):创建
		* 创建数据库：
			* create database 数据库名称;
		* 创建数据库，判断不存在，再创建：
			* create database if not exists 数据库名称;
		* 创建数据库，并指定字符集
			* create database 数据库名称 character set 字符集名;
 
		* 练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
			* create database if not exists db4 character set gbk;
	2. R(Retrieve)：查询
		* 查询所有数据库的名称:
			* show databases;
		* 查询某个数据库的字符集:查询某个数据库的创建语句
			* show create database 数据库名称;
	3. U(Update):修改
		* 修改数据库的字符集
			* alter database 数据库名称 character set 字符集名称;
	4. D(Delete):删除
		* 删除数据库
			* drop database 数据库名称;
		* 判断数据库存在，存在再删除
			* drop database if exists 数据库名称;
	5. 使用数据库
		* 查询当前正在使用的数据库名称
			* select database();
		* 使用数据库
			* use 数据库名称;
 
 
2. 操作表
	1. C(Create):创建
		1. 语法：
			create table 表名(
				列名1 数据类型1,
				列名2 数据类型2,
				....
				列名n 数据类型n
			);
			* 注意：最后一列，不需要加逗号（,）
			* 数据库类型：
				1. int：整数类型
					* age int,
				2. double:小数类型
					* score double(5,2)
				3. date:日期，只包含年月日，yyyy-MM-dd
				4. datetime:日期，包含年月日时分秒	 yyyy-MM-dd HH:mm:ss
				5. timestamp:时间错类型	包含年月日时分秒	 yyyy-MM-dd HH:mm:ss	
					* 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
 
				6. varchar：字符串
					* name varchar(20):姓名最大20个字符
					* zhangsan 8个字符  张三 2个字符
 
 
		* 创建表
			create table student(
				id int,
				name varchar(32),
				age int ,
				score double(4,1),
				birthday date,
				insert_time timestamp
			);
		* 复制表：
			* create table 表名 like 被复制的表名;	  	
	2. R(Retrieve)：查询
		* 查询某个数据库中所有的表名称
			* show tables;
		* 查询表结构
			* desc 表名;
	3. U(Update):修改
		1. 修改表名
			alter table 表名 rename to 新的表名;
		2. 修改表的字符集
			alter table 表名 character set 字符集名称;
		3. 添加一列
			alter table 表名 add 列名 数据类型;
		4. 修改列名称 类型
			alter table 表名 change 列名 新列别 新数据类型;
			alter table 表名 modify 列名 新数据类型;
		5. 删除列
			alter table 表名 drop 列名;
	4. D(Delete):删除
		* drop table 表名;
		* drop table  if exists 表名 ;
```
## 多表查询

### 内连接

![[多表查询_内连接_图1.png]]

![[多表查询_内连接_图2.png]]


### 外连接

![[多表查询_外连接_图1.png]]
![[多表查询_外连接_图2.png]]


### 自连接

一定要给表取别名！！！！

![[多表查询_自链接_图1.png]]

![[多表查询_自链接_图2.png]]

### 联合查询
![[多表查询_联合查询_图1.png]]

![[多表查询_联合查询_图2.png]]

### 子查询

![[多表查询_子查询_图1.png]]

#### 标量子查询
返回的结果式单个值(数字，字符串，日期等) ，最简单的形式，这种查询成为标量子查询
常用操作符: = < > > >= < <=

![[多表查询_子查询_图2.png]]

![[多表查询_子查询_图3.png]]

![[多表查询_子查询_图5.png]]

#### 列子查询

![[多表查询_子查询_图7.png]]

![[多表查询_子查询_图6.png]]

#### 行子查询

子查询返回的结果是一行 （可以是多列），这种子查询称为行子查询

常用的操作符: =,  <, >, IN,  NOT IN

![[多表查询_子查询_图8.png]]

#### 表子查询

子查询返回的结果是多行多列，这种子查询称为表子查询
常用操作符 ：  IN

![[多表查询_子查询_图9.png]]

![[多表查询_子查询_图10.png]]

### Using

using关键字的概念：

- 连接查询时如果是同名字段作为连接条件，using可以代替on出现（比on更好）
- using 是针对同名字段（using(id)===on A.id=B.id）
- using 关键字使用后会自动合并对应字段为一个
- using 可以同时使用多个字段作为条件

在平时，我们做关联表查询的时候一般是这样的

``` sql
select * from 表1 inner join 表2 on 表1.相同的列=表2.相同的列;
```

然后可以改成这样也是同样的效果

``` sql
select 表1的列 from 表1 inner join 表2 on 表1.相同的列=表2.相同的列
```

然后还可以改成这样
``` sql

select * from 表1 inner join 表2 using(相同的列);

```




## 事务

![[事务_图1.png]]

![[事务_图2.png]]

![[事务_图3.png]]

![[事务_图4.png]]

![[事务_图5.png]]

![[事务_图6.png]]

注意：事务隔离级别越高，数据越安全，但是性能越低

幻读的解决是要等待另一个事务中对数据的操作结束并提交了，才会让其他事务去操作，其他操作在那个事务没有提交时，若进行操作，也会阻塞住


# 进阶

![[存储引擎_图2.png]]

## 存储引擎


![[存储引擎_图1.png]]

![[存储引擎_图3.png]]

![[存储引擎_图4.png]]

``` bash
# 启动mysql in linux

systemctl start mysqld

# 获取第一次密码
grep 'temporary password' /var/log/mysqld.log

OZ9rxmgvfN,=

mysql -u root -p

# 修改用户密码权限
set global validate_password.policy = 0;
set global validate_password.length = 4;

# 设置密码
Alter USER 'root'@'localhost' IDENTIFIED BY '1234';

# 默认的root用户只能当前节点的localhost访问 是无法远程访问的，我们还需要创建一个root用户 进行远程访问
 create user 'root'@'%' IDENTIFIED WITH mysql_native_password by '1234';

```
## 索引

索引是一种更有序的数据结构，是用来高效获取数据的 。

索引 
优点
提高数据检索效率
通过索引对数据排列，降低数据排列的成本，降低CPU消耗
缺点
1 占空间
2 虽然提升了查询效率 但是也降低了更新表的速度，如 insert update delete 时 效率降低

索引 是由存储引擎实现的 ，不同的存储引擎有不同的结构

![[索引_图1.png]]

![[索引_图2.png]]


如果是二叉树的话
顺序插入时，就会形成一个链表 查询性能大大降低。大数据量的情况下 层级深，检索速度慢
特点 向上分裂 比如一个B树最大度数是5 一个节点最多五个指针，四个数据，当新数据加入时，如果这个节点已经有四个数据五个节点了 就向上分裂，五个数中的中间一个向上

![[索引_图3.png]]


### B+树

![[索引_图4.png]]

![[索引_图5.png]]

https://mp.weixin.qq.com/s?__biz=MzU0OTE4MzYzMw==&mid=2247505924&idx=4&sn=eae493ee38fdd204c3524af22fefdf2c&chksm=fbb151faccc6d8ec52026c9393316a5384765caa34828213b4db5e45886c26004162db985633&scene=27

### Hash索引

![[索引_图6.png]]

![[索引_图7.png]]

![[索引_图8.png]]

### 索引类型

![[索引_图9.png]]

![[索引_图10.png]]

![[索引_图11.png]]
### SQL 性能分析

``` sql
show global status like 'Com_______'
# 通过这句话可以查看当前数据库 insert update delete select 的访问频次
```
![[索引_图12.png]]

#### 慢日志查询

慢查询日志记录了所有执行时间超过 指定参数 （long_query_time,默认10s）的所有SQL语句的日志。

MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件里配置

```

# 开启MySQL慢日志查询开关
slow_query_log = 1

# 设置慢日志的时间为2秒，SQL语句的执行时间超过两秒 就会视为慢查询，记录慢查询日志
long_query_time = 2
```



配置完毕后 重启Mysq服务 查看慢日志文件中记录的信息
/var/lib/mysql/localhost-slow.log

### profile

![[索引_图13.png]]

![[索引_图15.png]]

查看query为16的 各个阶段执行的时间
![[索引_图14.png]]

### explain 执行方法

![[索引_explain_图1.png]]

多对多的关系需要靠中间表维护

![[索引_explain_图2.png]]

![[索引_explain_图3.png]]

所以一般需要重点关注 type possible_keys key rows Extra

### 索引使用

#### 最左前缀
![[索引_最左前缀_图1.png]]

![[索引_最左前缀_图2.png]]

#### 索引失效

1 不要再索引列上进行运算操作，否则索引失效

2 字符串类型的字段使用，不加引号

3 模糊查询时 如果仅仅是尾部模糊匹配，索引不会失效，如果是**头部模糊匹配，索引失效**


![[索引_索引失效_图1.png]]


4 or 连接的条件
用or分割开的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么设计的索引都不会被用到
![[索引_索引失效_图2.png]]


5 数据分布影响：
如果Mysql评估使用索引比全表更慢，则不使用索引

比如 表中某个字段没有null 值 ，此时用
``` sql
explain select * from 表名 where 字段名 is not null  
```
命令可以发现  是使用了索引，若用
``` sql
explain select * from 表名 where 字段名 is null  
```
 可以发现是没有用索引，因为这个字段没有null值

#### SQL提示

![[索引_sql提示_图1.png]]

### 覆盖索引 和 回表

回表：

当执行一个查询语句，包含了辅助索引的列时，MySQL会首先使用辅助索引定位到符合条件的记录的主键值，然后再根据这些主键值去主键索引查找对应的完整数据行。这个过程就被称为**回表**

using index condition 就代表回表查询了
using where; using index  查找使用了索引 但是需要的数据都在索引列中找到，所以不需要回表查询数据

![[索引_覆盖索引和回表_图1.png]]

所以一般会建议 尽量不要使用select * 这种形式，很容易出现回表，除非建立联合索引把所有的列字段都包含进去

### 前缀索引

![[索引_前缀索引_图1.png]]

第一个select方法是来计算 email字段里 去重后与总行数的比值
第二个select方法是用来计算 截取email字段里的值5位 去重后与总行数的比值

``` sql
create index idx_email_5 on tb_user(email(5));
```

创建tb_user表中 取email字段里前五个字符去建立索引

![[索引_前缀索引_图2.png]]

1 先去辅助索引中去找到 lvbu6开头的下标，在用这个下标去回表 找到对应的行信息。
2 再去看该行的email字段是否和找到的一致，一致的话先暂存，不一致则在去看辅助索引找到的lvbu6后面是否还有lvbu6， 有的话重复步骤1

### 单列索引和联合索引

单列索引 即一个索引只包含单列
联合索引 即一个索引包含多个列

![[索引_使用规则联合索引_图1.png]]

![[索引_使用规则联合索引_图2.png]]

单列索引和联合索引都存在的情况下 推荐使用联合索引去查找（通过指定索引的方式去使用），当然mysql本身有优化器，也 可能 会选择比较优的选择

### 索引设计原则

1 由于数据量较大，并且查询比较频繁的表建立索引
2 针对与常作为查询条件(where) order by 排序， 分组(group by)操作的字段建立索引
3 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高， 使用索引的效率越高
4 如果是字符串类型的字段，字段的长度较长， 可以针对于走低阿曼德特倒霉，建立前缀索引
5 尽量使用联合索引，减少单列索引，查询时 联合索引很多时候可以覆盖索引，节省存储空间 避免回表
6 索引不是多多益善 要控制索引的数量 索引越多，维护成本越大，也会影响增删改的效率
7 如果索引列不能存储NULL 请在建表时使用 NOT NULL 约束。当优化器知道每列是否包含NULL 时， 可以更好地确定哪个索引最有效地用于查询



## SQL 优化

https://mp.weixin.qq.com/s/CaSVhAJgycjjbCxAkII2ZA

![[SQL优化_补充_图1.png]]

![[SQL优化_补充_图2.png]]
![[SQL优化_补充_图3.png]]


![[SQL优化_补充_图4.png]]

![[SQL优化_补充_图5.png]]

### 知识点：索引下推

![[SQL优化_索引下推_图1.png]]


**索引下推使用条件**

- 只能用于`range`、 `ref`、 `eq_ref`、`ref_or_null`访问方法；
    
- 只能用于`InnoDB`和 `MyISAM`存储引擎及其分区表；
    
- 对存储引擎来说，索引下推只适用于二级索引（也叫辅助索引）;
    

索引下推的目的是为了减少回表次数，也就是要减少 IO 操作。对于的聚簇索引来说，数据和索引是在一起的，不存在回表这一说。

- 引用了子查询的条件不能下推；
    
- 引用了存储函数的条件不能下推，因为存储引擎无法调用存储函数。


### 插入数据

1 建议使用批量插入
 不建议一次性一条语句一次性插入1000条以上数据

2 建议使用 手动提交事务
多条insert 语句执行前开启事务 执行完之后统一提交事务

3 主键顺序插入

主键顺序插入的性能高于乱序插入

![[SQL优化_插入数据_图1.png]]


### 主键优化

InnoDB存储引擎中，表数据都是根据主键顺序存放的，这种存储方式的表称为索引组织表

![[索引_主键优化_图1.png]]

如上图所示，是一个主键乱序插入的过程，此时按理说 如果第一页空间足够的话，id为50的这一行数据是要放在第一页上的。但是第一页已经没有这么多空间去存放了，那么就会新开一个页去存放这个数据，并且从要准备存放的那一页 选取内存占比一半的数据，一起和新的数据放到新的一页上，也保证了一页不能只有一行数据。并且页也会重新排序 如下图所示
![[SQL优化_插入数据_图2.png]]

P.S. 如果按照上面的页分裂的思路的话 如果出现 一页只有一行数据，而新增的数据id正好紧跟其后，就可能会有问题。而且Mysql也设置了 一个行数据的最大大小为8KB

![[索引_主键优化_图3.png]]

![[索引_主键优化_图4.png]]

![[索引_主键优化_图5.png]]

#### order by优化

![[SQL优化_ORDERBY_图1.png]]

![[SQL优化_ORDERBY_图2.png]]

``` sql
explain select id, age, phone from tb_user order by desc, phone desc;

# 此时 索引也会生效，并且 extra会显示 backward index scan; using index

```

此时 索引还都有效

![[SQL优化_ORDERBY_图3.png]]

因此 可以创建 索引 age 升序， phone降序的

create index idx_user_age_pho_ad on tb_user(age asc, phone desc); 

通过这样创建，就可以使得 order by age asc, phone desc 生效

![[SQL优化_ORDERBY_图4.png]]

所以：
1 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
2 尽量使用覆盖索引 （遵循索引的优化）
3 多字段排序，一个升序 一个降序，此时需要注意联合索引在创建时的规则
4 如果 不可避免地出现filesort 大数据量排序时，可以上当增大排序缓冲区大小 sort_buffer_size（默认256k）
可以通过 show variables like 'sort_buffer_size'得到

#### group by 优化

![[SQL优化_groupby_图4.png]]

show index from 表名 
展示表内所有索引
![[SQL优化_GroupBy_图1.png]]

![[SQL优化_GroupBy_图2.png]]
https://dev.mysql.com/doc/refman/8.4/en/group-by-optimization.html

理论上看起来应该索引失效才对 但是为什么还会有Using index？

![[SQL优化_groupby_图3.png]]


#### limit优化

![[SQL优化_limit_图1.png]]

![[SQL优化_limit_图2.png]]

#### count优化

![[SQL优化_count优化_图4.png]]


#### update

由于InnoDB是行锁 是针对索引家的锁，不是针对记录加的锁！！
并且该索引不能失效！

更新要注意 一定要对索引字段进行更新，否则会将行锁升级为表锁
会导致其他事务该另一个字段的时候，表被锁住导致整个事务都被阻塞住！！！


## 视图 / 存储过程 / 触发器

### 视图

![[视图_图1.png]]

![[视图_图2.png]]

![[视图_图3.png]]

![[视图_cascaded_图1.png]]

![[视图_local_图1.png]]

![[视图_图4.png]]

### 存储过程



### 存储函数
![[存储过程_图1.png]]
![[存储过程_图2.png]]

#### 全局变量
用户自定义变量： @变量名
![[存储过程_图3.png]]

#### 用户自定义变量
![[存储过程_图4.png]]

![[存储过程_图5.png]]

#### IF

![[存储过程_图6.png]]

![[存储过程_图7.png]]



https://www.cnblogs.com/sx66/p/17881258.html


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



















