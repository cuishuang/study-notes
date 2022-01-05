---
title: golang之channel并发访问
date: 2019-01-19 19:38:23
tags: [Go,Pearl]
---

<br>



channel底层是加锁的，是一个有锁的环形队列。。故而肯定并发安全


可参考 [golang之channel进阶](https://dashen.tech/2020/10/15/golang%E4%B9%8Bchannel%E8%BF%9B%E9%98%B6/)



> Channel 在运行时的内部表示是 runtime.hchan，该结构体中包含了一个用于保护成员变量的互斥锁，从某种程度上说，Channel 是一个用于同步和通信的有锁队列。
<br>
有很多试图通过各种方式 实现 **无锁 Channel** 的方案,但目前都还有各种各样问题尚不够完美.
<br>
所以实际上也借助了锁 (sync.Mutex)






