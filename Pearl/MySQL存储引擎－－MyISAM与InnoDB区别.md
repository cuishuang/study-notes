---
title: MySQL存储引擎－－MyISAM与InnoDB区别
date: 2020-07-07 15:26:09
tags: [数据库,Interview,Pearl]
---


- InnoDB 支持·事务处理/外键/行级锁（共享锁和排他锁）·等高级处理，而MyISAM不支持；

- InnoDB 为聚簇索引，MyISAM为非聚簇索引；



以下是一些细节和具体实现的差别：

　　◆1.InnoDB不支持FULLTEXT类型的索引。

　　◆2.InnoDB 中不保存表的具体行数，也就是说，执行select count(`*`) from table时，InnoDB要扫描一遍整个表来计算有多少行，但是MyISAM只要简单的读出保存好的行数即可。注意的是，当count(`*`)语句包含 where条件时，两种表的操作是一样的。

　　◆3.对于AUTO_INCREMENT类型的字段，InnoDB中必须包含只有该字段的索引，但是在MyISAM表中，可以和其他字段一起建立联合索引。

　　◆4.DELETE FROM table时，InnoDB不会重新建立表，而是一行一行的删除。

　　◆5.LOAD TABLE FROM MASTER操作对InnoDB是不起作用的，解决方法是首先把InnoDB表改成MyISAM表，导入数据后再改成InnoDB表，但是对于使用的额外的InnoDB特性(例如外键)的表不适用。

　　另外，InnoDB表的行锁也不是绝对的，假如在执行一个SQL语句时MySQL不能确定要扫描的范围，InnoDB表同样会锁全表，例如update table set num=1 where name like “%aaa%”

---

参考：[MySQL存储引擎－－MyISAM与InnoDB区别](https://blog.csdn.net/xifeijian/article/details/20316775)




InnoDB在事务执行过程中，使用两阶段锁协议：

随时都可以执行锁定，InnoDB会根据隔离级别在需要的时候自动加锁；

锁只有在执行commit或者rollback的时候才会释放，并且所有的锁都是在同一时刻被释放。


[再谈mysql锁机制及原理—锁的诠释](https://juejin.im/post/5dac651451882529d1528e12)

[mysql锁机制浅析](https://segmentfault.com/a/1190000004507047)






<br>

1、MyISAM是非事务安全的，而InnoDB是事务安全的

2、MyISAM锁的粒度是表级的，而InnoDB支持行级锁

3、MyISAM支持全文类型索引，而InnoDB不支持全文索引

4、MyISAM相对简单，效率上要优于InnoDB，小型应用可以考虑使用MyISAM

5、MyISAM表保存成文件形式，跨平台使用更加方便

6、MyISAM管理非事务表，提供高速存储和检索以及全文搜索能力，如果在应用中执行大量select操作可选择

7、InnoDB用于事务处理，具有ACID事务支持等特性，如果在应用中执行大量insert和update操作，可选择。

