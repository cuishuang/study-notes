---
title: sync包-Mutex
date: 2019-05-08 23:46:49
tags: Go
categories: sync
---



### <font color="#CD853F">初入门径</font>

<br>


// A Mutex is a **mut**ual **ex**clusion lock.

Mutex是互斥锁

// The zero value for a Mutex is an unlocked mutex. 
 互斥锁的零值是未锁定的互斥锁


// A Mutex must not be copied after first use.

首次使用后不得复制Mutex


<font color="#228B22" size=1>


mutual	英[ˈmjuːtʃuəl]
美[ˈmjuːtʃuəl]

adj.	相互的; 彼此的; 共有的; 共同的;

[例句]The East and the West can work together for their mutual benefit and progress

东西方可以为彼此共同的利益和发展而合作。

<br>

exclusion	英[ɪkˈskluːʒn]
美[ɪkˈskluːʒn]

n.	排斥; 排除在外; 不包括在内的人(或事物); 被排除在外的人(或事物); 排除; 认为不可能;

[例句]It calls for the exclusion of all commercial lending institutions from the college loan program

它提倡将所有商贷机构排除在高校贷款计划之外。

</font>


---



<br>


### <font color="#CD853F">源码实现</font>


<br>


[go中semaphore源码解读](https://boilingfrog.github.io/2021/04/02/semaphore/)

[Go精妙的互斥锁设计](https://mp.weixin.qq.com/s/YYvoeDfPMm8Y2kFu9uesGw)

[golang-浅析mutex](https://zhuanlan.zhihu.com/p/340536378)

[Go语言sync包的应用详解](https://mp.weixin.qq.com/s/l315emdX2LayvQtMRMxigA)

[semaphore 的原理与实现](https://mp.weixin.qq.com/s/GB649snXQ5rDF2BXO9V55Q)

[Golang 语言中基础同步原语 Mutex 和 RWMutex 的区别](https://mp.weixin.qq.com/s/nU-WFszIP1Sk1rC4q5cI1A)

[手摸手Go 并发编程基建Semaphore](https://mp.weixin.qq.com/s/7OZ8lmkiQU16884owQR-fw)

[缓存击穿导致 golang 组件死锁的问题分享](https://mp.weixin.qq.com/s/LhP4Yom1NdgmQi-0kwkRfQ)




<br>

---

<br>



### <font color="#CD853F">官方库或知名项目中的使用</font>

<br>




---



<br>

参考:


[GO: sync.Mutex 的实现与演进](https://www.jianshu.com/p/ce1553cc5b4f)


[Golang同步机制的实现](http://ga0.github.io/golang/2015/10/11/golang-sync.html)



[golang-浅析mutex](https://zhuanlan.zhihu.com/p/340536378)