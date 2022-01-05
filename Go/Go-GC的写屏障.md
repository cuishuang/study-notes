---
title: Go GC的写屏障
date: 2012-11-03 21:12:44
tags: Go
categories: runtime
---


https://github.com/golang-design/under-the-hood/issues/20


http://www.icewater.xyz/p/go-gc/



https://github.com/golang-design/under-the-hood/issues/79



https://zhuanlan.zhihu.com/p/74853110


https://www.bookstack.cn/read/qcrao-Go-Questions/spilt.6.GC-GC.md


https://golang.design/under-the-hood/zh-cn/part2runtime/ch08gc/barrier/


https://jishuin.proginn.com/p/763bfbd641c8


https://segmentfault.com/a/1190000022030353

https://zhuanlan.zhihu.com/p/340455930




https://www.bookstack.cn/read/For-learning-Go-Tutorial/src-spec-02.0.md


https://www.jianshu.com/p/52ab0bebefa0


http://www.yuasa.kuis.kyoto-u.ac.jp/~yuasa/


https://dblp.org/pid/30/3398.html

https://jishuin.proginn.com/p/763bfbd4ec55


https://mp.weixin.qq.com/s/iklfWLmSD4XMAKmFcffp9g




```go
Go uses a hybrid barrier that combines a Yuasa-style deletion barrier—which shades the object whose reference is being overwritten—with Dijkstra insertion barrier—which shades the object whose reference is being written. The insertion part of the barrier is necessary while the calling goroutine's stack is grey. In pseudocode, the barrier is:     writePointer(slot, ptr):         shade(*slot)         if current stack is grey:             shade(ptr)         *slot = ptr slot is the destination in Go code. ptr is the value that goes into the slot in Go code. Shade indicates that it has seen a white pointer by adding the referent to wbuf as well as marking it. The two shades and the condition work together to prevent a mutator from hiding an object from the garbage collector: 1. shade(*slot) prevents a mutator from hiding an object by moving the sole pointer to it from the heap to its stack. If it attempts to unlink an object from the heap, this will shade it. 2. shade(ptr) prevents a mutator from hiding an object by moving the sole pointer to it from its stack into a black object in the heap. If it attempts to install the pointer into a black object, this will shade it. 3. Once a goroutine's stack is black, the shade(ptr) becomes unnecessary. shade(ptr) prevents hiding an object by moving it from the stack to the heap, but this requires first having a pointer hidden on the stack. Immediately after a stack is scanned, it only points to shaded objects, so it's not hiding anything, and the shade(*slot) prevents it from hiding any other pointers on its stack. For a detailed description of this barrier and proof of correctness, see https://github.com/golang/proposal/blob/master/design/17503-eliminate-rescan.md Dealing with memory ordering: Both the Yuasa and Dijkstra barriers can be made conditional on the color of the object containing the slot. We chose not to make these conditional because the cost of ensuring that the object holding the slot doesn't concurrently change color without the mutator noticing seems prohibitive. Consider the following example where the mutator writes into a slot and then loads the slot's mark bit while the GC thread writes to the slot's mark bit and then as part of scanning reads the slot. Initially both [slot] and [slotmark] are 0 (nil) Mutator thread          GC thread st [slot], ptr          st [slotmark], 1 ld r1, [slotmark]       ld r2, [slot] Without an expensive memory barrier between the st and the ld, the final result on most HW (including 386/amd64) can be r1==r2==0. This is a classic example of what can happen when loads are allowed to be reordered with older stores (avoiding such reorderings lies at the heart of the classic Peterson/Dekker algorithms for mutual exclusion). Rather than require memory barriers, which will slow down both the mutator and the GC, we always grey the ptr object regardless of the slot's color. Another place where we intentionally omit memory barriers is when accessing mheap_.arena_used to check if a pointer points into the heap. On relaxed memory machines, it's possible for a mutator to extend the size of the heap by updating arena_used, allocate an object from this new region, and publish a pointer to that object, but for tracing running on another processor to observe the pointer but use the old value of arena_used. In this case, tracing will not mark the object, even though it's reachable. However, the mutator is guaranteed to execute a write barrier when it publishes the pointer, so it will take care of marking the object. A general consequence of this is that the garbage collector may cache the value of mheap_.arena_used. (See issue #9984.) Stack writes: The compiler omits write barriers for writes to the current frame, but if a stack pointer has been passed down the call stack, the compiler will generate a write barrier for writes through that pointer (because it doesn't know it's not a heap pointer). One might be tempted to ignore the write barrier if slot points into to the stack. Don't do it! Mark termination only re-scans frames that have potentially been active since the concurrent scan, so it depends on write barriers to track changes to pointers in stack frames that have not been active. Global writes: The Go garbage collector requires write barriers when heap pointers are stored in globals. Many garbage collectors ignore writes to globals and instead pick up global -> heap pointers during termination. This increases pause time, so we instead rely on write barriers for writes to globals so that we don't have to rescan global during mark termination. Publication ordering: The write barrier is *pre-publication*, meaning that the write barrier happens prior to the *slot = ptr write that may make ptr reachable by some goroutine that currently cannot reach it. Signal handler pointer writes: In general, the signal handler cannot safely invoke the write barrier because it may run without a P or even during the write barrier. There is exactly o
```