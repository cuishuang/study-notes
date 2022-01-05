---
title: sync包-Waitgroup实现原理
date: 2017-07-25 18:27:29
tags: Go
categories: sync
---

<br>


### <font color="#CD853F">初入门径</font>


<br>

可以等待一组 Goroutine 的返回，一个比较常见的使用场景是批量发出 RPC 或者 HTTP 请求.


`sync.WaitGroup`实现很简单，只有100多行的代码; 底层使用**计数器**和**信号量**来实现同步。


```java
Semaphore

n.	信号标; 旗语;
v.	打旗语; (用其他类似的信号系统) 发信号;
[例句]The definition of a shared memory and process 
shared data structure and built-in semaphore support.
有一个共享的内存定义和进程共享数据结构和内置的信号量的支持。


acquire 
v. 获得; 购得; 获得; 得到;


release	
v.	释放; 放出; 放走; 放开; 松开; 发泄; 宣泄;

```

<br>

如 php中的[sem_acquire](https://www.php.net/manual/en/function.sem-acquire.php)函数

<br>


参考:


[sync.WaitGroup实现原理详解](https://memosa.cn/golang/2019/10/31/golang-waitgroup.html)

[Golang的sync.WaitGroup 实现逻辑和源码解析](https://www.cnblogs.com/jiangz222/p/10348763.html)

[golang sync.WaitGroup 底层实现](http://yangxikun.github.io/golang/2020/02/15/golang-sync-waitgroup.html)


`>>>`

<br>


### <font color="#CD853F">源码实现</font>


<br>


 **sync.WaitGroup**对外暴露了三个方法 — `sync.WaitGroup.Add、sync.WaitGroup.Wait 和 sync.WaitGroup.Done`


<br>


```go
// A WaitGroup waits for a collection of goroutines to finish.
// The main goroutine calls Add to set the number of
// goroutines to wait for. Then each of the goroutines
// runs and calls Done when finished. At the same time,
// Wait can be used to block until all goroutines have finished.
//
// A WaitGroup must not be copied after first use.

// WaitGroup等待goroutine的集合完成。主goroutine调用Add来设置等待的goroutines。 然后每goroutines运行并在完成后调用完成。 同时，等待可用于阻塞，直到所有goroutine完成。
//首次使用后不得复制WaitGroup。

type WaitGroup struct {
	noCopy noCopy

	// 64-bit value: high 32 bits are counter, low 32 bits are waiter count.
	// 64-bit atomic operations require 64-bit alignment, but 32-bit
	// compilers do not ensure it. So we allocate 12 bytes and then use
	// the aligned 8 bytes in them as state, and the other 4 as storage
    // for the sema.
    
    // 64位值：高32位为计数器，低32位为waiter计数。 64位原子操作需要64位对齐，但是32位编译器不能确保对齐。 因此，我们分配12个字节，然后将对齐的8个字节用作状态，并将其他4个用作存储信号。
	state1 [3]uint32
}

```

- noCopy — 保证 sync.WaitGroup 不会被开发者通过再赋值的方式拷贝；
- state1 — 存储着状态和信号量；

[源码地址](https://github.com/golang/go/blob/71239b4f491698397149868c88d2c851de2cd49b/src/sync/waitgroup.go#L20-L29)


<br>

```go
// state returns pointers to the state and sema fields stored within wg.state1.
// state返回指向存储在wg.state1中的state和sema字段的指针。
func (wg *WaitGroup) state() (statep *uint64, semap *uint32) {
	if uintptr(unsafe.Pointer(&wg.state1))%8 == 0 {
		return (*uint64)(unsafe.Pointer(&wg.state1)), &wg.state1[2]
	} else {
		return (*uint64)(unsafe.Pointer(&wg.state1[1])), &wg.state1[0]
	}
}
```



<br>



#### <font color="#FF6347">sync.WaitGroup.Add</font>



<br>

```go

// Add adds delta, which may be negative, to the WaitGroup counter.
// If the counter becomes zero, all goroutines blocked on Wait are released.
// If the counter goes negative, Add panics.
//
// Note that calls with a positive delta that occur when the counter is zero
// must happen before a Wait. Calls with a negative delta, or calls with a
// positive delta that start when the counter is greater than zero, may happen
// at any time.
// Typically this means the calls to Add should execute before the statement
// creating the goroutine or other event to be waited for.
// If a WaitGroup is reused to wait for several independent sets of events,
// new Add calls must happen after all previous Wait calls have returned.
// See the WaitGroup example.

// Add将增量（可能为负）添加到WaitGroup计数器中。
//如果计数器变为零，则释放处于等待阻塞状态的所有goroutine。
//如果计数器变为负数，则抛出panic。
//
// 请注意，当计数器为零时发生的增量为正的调用必须在等待之前发生。 在计数器大于零时开始的负增量呼叫或正增量呼叫可能随时发生。
// 通常，这意味着对Add的调用应在创建goroutine或要等待的其他事件的语句之前执行。
// 如果重新使用WaitGroup来等待几个独立的事件集，则必须在所有先前的Wait调用返回之后再进行新的Add调用。
// 请参见WaitGroup示例。
func (wg *WaitGroup) Add(delta int) {
	statep, semap := wg.state()
	if race.Enabled {
		_ = *statep // trigger nil deref early
		if delta < 0 {
			// Synchronize decrements with Wait.
			//和Wait同步减少量
			race.ReleaseMerge(unsafe.Pointer(wg))
		}
		race.Disable()
		defer race.Enable()
	}
	state := atomic.AddUint64(statep, uint64(delta)<<32)
	v := int32(state >> 32)
	w := uint32(state)
	if race.Enabled && delta > 0 && v == int32(delta) {
		// The first increment must be synchronized with Wait.
		// Need to model this as a read, because there can be
		// several concurrent wg.counter transitions from 0.

		//第一个增量必须与Wait同步。
		//需要将此模型建模为读取，因为从0开始可能会有多个并发的wg.counter转换。
		race.Read(unsafe.Pointer(semap))
	}

    //sync.WaitGroup.Add 方法可以更新 sync.WaitGroup 中的计数器 counter。
	//sync.WaitGroup.Add 方法传入的参数可以为负数，但是计数器只能是非负数，一旦出现负数就会发生程序崩溃
	if v < 0 {
		panic("sync: negative WaitGroup counter")
	}
	if w != 0 && delta > 0 && v == int32(delta) {
		//WaitGroup滥用：Add与Wait被并发调用
		panic("sync: WaitGroup misuse: Add called concurrently with Wait")
	}
	if v > 0 || w == 0 {
		return
	}
	// This goroutine has set counter to 0 when waiters > 0.
	// Now there can't be concurrent mutations of state:
	// - Adds must not happen concurrently with Wait,
	// - Wait does not increment waiters if it sees counter == 0.
	// Still do a cheap sanity check to detect WaitGroup misuse.

	//当waiters>0时，此goroutine已将计数器设置为0。
    //现在不可能有并发的状态突变：
    // -添加不得与等待同时进行，
    // -Wait看到counter == 0时，Wait不会增加waiters的数量。
    //仍然进行了一次廉价且高效的检查以检测防止WaitGroup的滥用。
	if *statep != state {
		panic("sync: WaitGroup misuse: Add called concurrently with Wait")
	}
	// Reset waiters count to 0.
	// 重置waiters的数量为0
	//当调用计数器归零，也就是所有任务都执行完成时，就会通过 sync.runtime_Semrelease 唤醒处于等待状态的所有 Goroutine。
	*statep = 0
	for ; w != 0; w-- {
		runtime_Semrelease(semap, false, 0)

		//release 释放,宣泄
	}
}

```


<br>


#### <font color="#FF6347">sync.WaitGroup.Done</font>

<br>

> sync.WaitGroup.Done 只是向 sync.WaitGroup.Add 方法传入了 -1


```go
// Done decrements the WaitGroup counter by one.
func (wg *WaitGroup) Done() {
	wg.Add(-1)
}
```

<br>

#### <font color="#FF6347">sync.WaitGroup.Wait</font>

<br>


> sync.WaitGroup 的另一个方法 sync.WaitGroup.Wait 会在计数器大于 0 并且不存在等待的 Goroutine 时，调用 sync.runtime_Semacquire 陷入睡眠状态。


```go
// Wait blocks until the WaitGroup counter is zero.
func (wg *WaitGroup) Wait() {
	statep, semap := wg.state()
	if race.Enabled {
		_ = *statep // trigger nil deref early
		race.Disable()
	}
	for {
		state := atomic.LoadUint64(statep)
		v := int32(state >> 32)
		w := uint32(state)
		if v == 0 {
			// Counter is 0, no need to wait.
			if race.Enabled {
				race.Enable()
				race.Acquire(unsafe.Pointer(wg))
			}
			return
		}
		// Increment waiters count.
		if atomic.CompareAndSwapUint64(statep, state, state+1) {
			if race.Enabled && w == 0 {
				// Wait must be synchronized with the first Add.
				// Need to model this is as a write to race with the read in Add.
				// As a consequence, can do the write only for the first waiter,
				// otherwise concurrent Waits will race with each other.

				//Wait必须与第一个Add同步。
				//需要将此建模为与Add中的读取竞争的写入。
				//因此，只能为第一个waiter执行写操作，否则并发的Waits将相互竞争。
				race.Write(unsafe.Pointer(semap))
			}
			runtime_Semacquire(semap)
			//acquire 获取,获得
			if *statep != 0 {
				panic("sync: WaitGroup is reused before previous Wait has returned")
			}
			if race.Enabled {
				race.Enable()
				race.Acquire(unsafe.Pointer(wg))
			}
			return
		}
	}
}

```



当 sync.WaitGroup 的计数器归零时，当陷入睡眠状态的 Goroutine 就被唤醒，上述方法会立刻返回。


<br>


### <font color="#CD853F">注意事项</font>

<br>

- sync.WaitGroup 必须在 sync.WaitGroup.Wait 方法返回之后才能被重新使用；

- sync.WaitGroup.Done 只是对 sync.WaitGroup.Add 方法的简单封装，我们可以向 sync.WaitGroup.Add 方法传入任意负数（需要保证计数器非负）快速将计数器归零以唤醒其他等待的 Goroutine；

- 可以同时有多个 Goroutine 等待当前 sync.WaitGroup 计数器的归零，这些 Goroutine 会被同时唤醒；






---

<br>


参考:

[图解Go里面的WaitGroup了解编程语言核心实现源码](https://blog.csdn.net/qq_42952272/article/details/103703159)

[同步原语与锁](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-sync-primitives/#waitgroup)