---
title: MySQL常用表级操作
date: 2017-09-29 21:16:09
tags: 数据库
---


### <font color="#008080">基础信息相关</font>

<br>


1.修改表名：

<br>

`rename table 旧表名 to 新表名;`

<br>

2、修改字段类型：

<br>

`alter table 表名 modify column 字段名 字段类型(长度)`

<br>

3、修改字段名称和类型：

<br>

`alter table 表名 change 现有字段名称  修改后字段名称 数据类型`

<br>


4、增加字段：

<br>

`alter table 表名 add 字段名 字段类型（长度）`

<br>

批量增加字段

`alter table 表名 add (字段名1 字段类型（长度）,字段名2 字段类型（长度）,...)`

<br>

5、删除字段：

<br>

`alter table 表名 drop column 字段名`

<br>

批量删除字段

`alter table 表名 drop column 字段名1，drop column 字段名2`

<br>


6、修改字段默认值：


<br>

`alter table 表名 alter column 字段 set default  默认值`


<br>


7、添加字段备注：

<br>

**alter table 表名 ~~add~~ modify 字段名 字段类型（长度）default null comment '备注'**


<br>


*为表添加注释*

`alter table 表名 comment '注释'`



<br>

---

<br>



### <font color="#008080"> 索引相关</font>

<br>


注: 索引名称是可选字段~

<br>

1.普通索引   添加index

<br>

`alter table `表名` add index 索引名称 (`字段名`)`

<br>


2.主键索引   添加primary key

<br>

`alter table `表名` add primary key (`字段名`)`

<br>



3.唯一索引    添加unique

<br>

`alter table `表名` add unique 索引名称 (`字段名`)`

<br>



4.全文索引    添加fulltext

<br>


`alter table `表名` add fulltext 索引名称 (`字段名`)`

<br>


5.如何添加多列索引

<br>

`alter table `表名` add index 索引名称 (`字段名`, `字段名`, `字段名`)`