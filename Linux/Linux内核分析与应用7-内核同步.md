---
title: Linux内核分析与应用7-内核同步
date: 2020-05-17 19:52:42
tags: Linux
---


<img src="Linux内核分析与应用7-内核同步/0.png" width = 100% height = 100% />


<br>


### 7.1 Linux同步概述

<br>


竞态条件,也称竞争条件,race condition


临界区

<img src="Linux内核分析与应用7-内核同步/1.png" width = 100% height = 100% />




<img src="Linux内核分析与应用7-内核同步/2.png" width = 100% height = 100% />




原子操作,Linux专门有一个atomic_t结构体


<img src="Linux内核分析与应用7-内核同步/3.png" width = 100% height = 100% />


问题:

在多核系统中遇到原子操作,在系统层面上原子操作还是原子的吗?在核级还是原子的吗?




<img src="Linux内核分析与应用7-内核同步/4.png" width = 100% height = 100% />



死锁:所有的任务都在相互等待,但他们永远不会释放已占有的资源,于是任何任务都无法继续执行

死锁的避免: 加锁的顺序是关键

<img src="Linux内核分析与应用7-内核同步/5.png" width = 100% height = 100% />


<br>

**思考:**

死锁是一种小概率事件还是大概率事件?如果内核出现死锁,该如何应对?




---

 
<br>


### 7.2 内核同步机制

<br>


<img src="Linux内核分析与应用7-内核同步/6.png" width = 100% height = 100% />


原子操作已经讲过.


中断屏蔽:

<img src="Linux内核分析与应用7-内核同步/7.png" width = 100% height = 100% />


<br>

自旋锁:(spin lock)

专为多处理器并发而引入的一种锁,在内核中大量应用于中断处理部分


在短期时间内,进行轻量级的锁定 


同一时刻,只能为一个处理器所持有, 防止多处理器并发访问临界区,防止内核抢占造成的竞争

<br>

信号量:

<img src="Linux内核分析与应用7-内核同步/8.png" width = 100% height = 100% />




<img src="Linux内核分析与应用7-内核同步/9.png" width = 100% height = 100% />


P/V操作


<img src="Linux内核分析与应用7-内核同步/10.png" width = 100% height = 100% />


<img src="Linux内核分析与应用7-内核同步/11.png" width = 100% height = 100% />
<img src="Linux内核分析与应用7-内核同步/12.png" width = 100% height = 100% />






经典实例: 生产者-消费者并发实例



<br>

参考:

[内核中的调度与同步](http://wwww.kerneltravel.net/journal/vi/syn.htm)


---

 
<br>


### 7.3 动手实践-内核多任务并发实例(上)

<br>



---

 
<br>


### 7.4 动手实践-内核多任务并发实例(下)

<br>