---
title: MySQL根据某张表复制新表
date: 2017-10-21 13:50:46
tags: 数据库
---


在[InnoDB一棵B+树,可以存放多少行数据](https://dashen.tech/2019/07/29/InnoDB%E4%B8%80%E6%A3%B5B-%E6%A0%91-%E5%8F%AF%E4%BB%A5%E5%AD%98%E6%94%BE%E5%A4%9A%E5%B0%91%E8%A1%8C%E6%95%B0%E6%8D%AE/)中,探讨了在B+数深度为3的前提下, mysql单表的数据上限:

如果单条记录大小为1k,能存`1170*1170*16=21902400`行数据. 实践中,可能单条记录会小于1k,从而单表上限可以稍稍增大,但一般也不超过5000万.


<br>


当某张表数据过大,就需要考虑从业务上进行分表,即将原来的单表a,拆成n张(如拆成100张a_00,a_01...a_99), 再根据业务的某项标识(如用户的uid),在代码中先决定写或读落到哪张表.

<br>


在单表扩容过程中,如下几条sql,将很有用处:

<br>


**复制一张新的user表,命名为user6. 仅复制表结构,不复制表数据**


```sql
create table user6 like user 
```

或

```sql
create table user6 select * from user where 1=2
```

<br>

如果去掉`where`条件,即


```sql
create table user6 select * from user 
```

则将会连原来user表的数据一同复制


---


<br>



**将某张表(user)的数据,复制写入另一张新表(user5)**

<br>

```sql
INSERT INTO user5 SELECT * FROM user;
```

