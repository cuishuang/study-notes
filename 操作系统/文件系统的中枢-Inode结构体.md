---
title: 文件系统的中枢---Inode结构体
date: 2020-06-10 20:26:10
tags: [Linux,操作系统,Pearl]
---

Inode记录了文件的元数据:

- 类型
- 权限
- 拥有者
- 各种时间(上次创建/修改内容/访问 时间)
- 连接数(一个文件只能有一个连接,但一个连接可以对用多个文件)
- 文件内容所在的位置(磁盘上`块`(block) 的下标)

一般文件系统的Inode大小是128或256Byte.

Inode以数组形式存放,每个元素都是一个Inode.

File System还会维护一个映射表(Map),存储 文件名 和 Inode的下标 之间的对应关系.


先根据文件名,去Map中找到对应Inode的index,再去存储Inode的数组中,去找到Value即Inode

<br>


扇区(sector)出厂之后就是固定的,早期老的磁盘 扇区一般是512Byte,现在新的磁盘,一般都是4K.

一个块(block),包含2的n次方个扇区(即必须是2的幂次方个)

块 过大或过小都不好,需要权衡.


当有非常非常多小文件时,可能出现`Inode`用完的现象,即便磁盘还有空间,也无法继续存储信息

(如Docker overlay 用尽inode...)




[阮一峰-理解inode](https://www.ruanyifeng.com/blog/2011/12/inode.html)