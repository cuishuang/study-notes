---
title: Linux内核分析与应用5-中断
date: 2020-05-17 19:52:21
tags: Linux
---

<img src="Linux内核分析与应用5-中断/0.png" width = 100% height = 100% />

<br>


### 中断机制概述

<br>

**中断是CPU对系统发生的某个事件作出的一种反应**, 当中断发生时,CPU暂停正在执行的程序,保留现场后,自动转去执行相应事件的处理程序,处理完成后,返回断点,继续执行被打断的程序.


<font color="orange">中断是操作系统的脉搏,是并发处理的基础.</font>


中断的引入,是为了支持CPU和设备之间的并行操作.


<br>

<img src="Linux内核分析与应用5-中断/1.png" width = 100% height = 100% />

<img src="Linux内核分析与应用5-中断/2.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/3.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/4.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/5.png" width = 100% height = 100% />


`中断`看似简单,但工程性非常强

---


 
<br>


### 5.2 中断处理机制

<br>

0x80,系统门的编号

<img src="Linux内核分析与应用5-中断/6.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/7.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/8.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/9.png" width = 100% height = 100% />

<img src="Linux内核分析与应用5-中断/10.png" width = 100% height = 100% />


<img src="Linux内核分析与应用5-中断/11.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/12.png" width = 100% height = 100% />




**思考:**

"中断返回"除了返回现场外,从源代码角度分析内核还做了什么?


---


 
<br>


### 5.3 中断下半部处理机制

<br>

软中断机制

<img src="Linux内核分析与应用5-中断/13.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/14.png" width = 100% height = 100% />


<br>

小任务(tasklet)机制


<img src="Linux内核分析与应用5-中断/15.png" width = 100% height = 100% />


**思考:**

1. 为什么要有中断下半部分处理机制?而且有好几种机制?

2. 中断下半部分处理机制中,你认为是否还有改进的余地?



---


 
<br>


### 5.4 时钟中断机制

<br>

<img src="Linux内核分析与应用5-中断/16.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/17.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/18.png" width = 100% height = 100% />
<img src="Linux内核分析与应用5-中断/19.png" width = 100% height = 100% />


在考察了如基树树,哈希表等多种数据结构后, hrtimer使用了红黑树(rbtree). 树最左边的节点是最快到期的时间

<img src="Linux内核分析与应用5-中断/20.png" width = 100% height = 100% />


**在内核中,除了被广泛使用的双向链表,红黑树使用场景也非常多.**



<img src="Linux内核分析与应用5-中断/21.png" width = 100% height = 100% />

<br>


**思考:**

1. 系统有了低精度定时器,为什么还要有高精度定时器?

2. 二者之间兼容么,在实现上有关系吗?


---


 
<br>


### 5.5 动手实践(上)

<br>




---


 
<br>


### 5.6 动手实践(下)

<br>