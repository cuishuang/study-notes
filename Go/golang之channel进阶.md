---
title: golang之channel进阶
date: 2020-10-15 12:20:10
tags: Go
---


Channel 在运行时的内部表示是 runtime.hchan，该结构体中包含了一个用于保护成员变量的互斥锁，从某种程度上说，Channel 是一个用于同步和通信的有锁队列。

有很多试图通过各种方式 实现 **无锁 Channel** 的方案,但目前都还有各种各样问题尚不够完美.


所以实际上也借助了锁 (sync.Mutex)


Go 语言的 Channel 在运行时使用 runtime.hchan 结构体表示。我们在 Go 语言中创建新的 Channel 时，实际上创建的都是该结构体:


[src/runtime/chan.go]()

```go
type hchan struct {
	qcount   uint           // total data in the queue
	dataqsiz uint           // size of the circular queue
	buf      unsafe.Pointer // points to an array of dataqsiz elements
	elemsize uint16
	closed   uint32
	elemtype *_type // element type
	sendx    uint   // send index
	recvx    uint   // receive index
	recvq    waitq  // list of recv waiters
	sendq    waitq  // list of send waiters

	// lock protects all fields in hchan, as well as several
	// fields in sudogs blocked on this channel.
	//
	// Do not change another G's status while holding this lock
	// (in particular, do not ready a G), as this can deadlock
	// with stack shrinking.
	lock mutex
}

```


<br>


[Go Channel 应用模式](https://colobu.com/2018/03/26/channel-patterns/)




