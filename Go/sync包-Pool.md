---
title: sync包-Pool
date: 2017-10-20 19:09:09
tags: Go
categories: sync
---

<br>


```go
// A Pool is a set of temporary objects that may be individually saved and
// retrieved.
//
// Any item stored in the Pool may be removed automatically at any time without
// notification. If the Pool holds the only reference when this happens, the
// item might be deallocated.
//
// A Pool is safe for use by multiple goroutines simultaneously.
//
// Pool's purpose is to cache allocated but unused items for later reuse,
// relieving pressure on the garbage collector. That is, it makes it easy to
// build efficient, thread-safe free lists. However, it is not suitable for all
// free lists.
//
// An appropriate use of a Pool is to manage a group of temporary items
// silently shared among and potentially reused by concurrent independent
// clients of a package. Pool provides a way to amortize allocation overhead
// across many clients.
//
// An example of good use of a Pool is in the fmt package, which maintains a
// dynamically-sized store of temporary output buffers. The store scales under
// load (when many goroutines are actively printing) and shrinks when
// quiescent.
//
// On the other hand, a free list maintained as part of a short-lived object is
// not a suitable use for a Pool, since the overhead does not amortize well in
// that scenario. It is more efficient to have such objects implement their own
// free list.
//
// A Pool must not be copied after first use.
type Pool struct {
	noCopy noCopy

	local     unsafe.Pointer // local fixed-size per-P pool, actual type is [P]poolLocal
	localSize uintptr        // size of the local array

	victim     unsafe.Pointer // local from previous cycle
	victimSize uintptr        // size of victims array

	// New optionally specifies a function to generate
	// a value when Get would otherwise return nil.
	// It may not be changed concurrently with calls to Get.
	New func() interface{}
}
```


<br>


---

<br>

源码分析:


[深度解密Go语言之sync.pool](https://juejin.cn/post/6918378263345627149)

[Go sync.Pool 浅析](https://mp.weixin.qq.com/s/MTf_4WEvCnCMKeQC_Wm_xw)

[理解Go 1.13中sync.Pool的设计与实现](https://segmentfault.com/a/1190000021944703)

[Go 1.13中 sync.Pool 是如何优化的?](https://colobu.com/2019/10/08/how-is-sync-Pool-improved-in-Go-1-13/)


