---
title: Mutex vs Atomic
date: 2017-08-25 20:02:35
tags: Go
categories: sync
---


本篇是对[Golang中的CAS原子操作 和 锁](https://blog.haohtml.com/archives/25881)的学习与记录

<br>

### 概况：

<br>

在高并发场景下，对同一变量并发写操作时，需要使用 锁 或 CAS原子操作来保证最终结果的正确。此文比较二者的差异


<br>



比如起10000个协程，对同一变量进行+1操作。如果不采取任何**措施**(无锁&&无CAS操作)，则会出现问题 (与预期结果不一致，且每次执行的结果都不同)


<br>


#### 无锁&无CAS：

<br>

`noLockandCas.go`
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var count int64
	var wg sync.WaitGroup

	for i := 0; i < 10000; i++ {
		wg.Add(1)

		go func(wg *sync.WaitGroup) {
			defer wg.Done()
			count = count + 1
		}(&wg)
	}

	wg.Wait()

	fmt.Println("count is:", count) //count != 10000

}
```

<br>

多次执行，结果为：

```go
count is: 8820
count is: 9336
count is: 8976
```


<br>


#### 加锁的实现方式

<br>


```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var count int64
	var wg sync.WaitGroup
	var mu sync.Mutex

	for i := 0; i < 10000; i++ {
		wg.Add(1)

		go func(wg *sync.WaitGroup) {
			defer wg.Done()
			mu.Lock()
			count = count + 1
			mu.Unlock()
		}(&wg)
	}

	wg.Wait()


	fmt.Println("count is:",count) //count = 10000

}
```

<br>

多次执行，结果为：

```go
count is: 10000
count is: 10000
count is: 10000
```
<br>




#### atomic CAS 原子操作

<br>

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var count int64
	var wg sync.WaitGroup

	for i := 0; i < 10000; i++ {
		wg.Add(1)

		go func(wg *sync.WaitGroup) {
			defer wg.Done()
			// 失败一直重试
			for {
				old := atomic.LoadInt64(&count)
				if atomic.CompareAndSwapInt64(&count, old, old+1) {
					break
				}
			}
		}(&wg)
	}

	wg.Wait()

	fmt.Println("count is:", count) //count = 10000

}

```

<br>

多次执行，结果为：

```go
count is: 10000
count is: 10000
count is: 10000
```
<br>





<br>

### 性能差异：

<br>


**可以看到，加锁或使用CAS，都可以达到目的。 来比较下二者的性能差异**

<br>


`lockandcat_test.go:`

```go
package main

import (
	"sync"
	"sync/atomic"
	"testing"
)

func lockTest() {
	var count int64
	var wg sync.WaitGroup
	var mu sync.Mutex

	for i := 0; i < 10000; i++ {
		wg.Add(1)
		go func(wg *sync.WaitGroup) {
			defer wg.Done()
			mu.Lock()
			count = count + 1
			mu.Unlock()
		}(&wg)
	}
	wg.Wait()
}

func atomicTest() {
	var count int64
	var wg sync.WaitGroup

	for i := 0; i < 10000; i++ {
		wg.Add(1)
		go func(wg *sync.WaitGroup) {
			defer wg.Done()
			// 失败一直重试
			for {
				old := atomic.LoadInt64(&count)
				if atomic.CompareAndSwapInt64(&count, old, old+1) {
					break
				}
			}
		}(&wg)
	}
	wg.Wait()
}

func BenchmarkLock(b *testing.B) {
	for i := 0; i < b.N; i++ {
		lockTest()
	}
}

func BenchmarkAtomic(b *testing.B) {
	for i := 0; i < b.N; i++ {
		atomicTest()
	}
}
```

<br>


#### 单个CPU

<br>

`go test -bench=".*" -v -benchmem -cpu=1`


```go
goos: darwin
goarch: arm64
pkg: xxxxx
BenchmarkLock
BenchmarkLock                548           2002265 ns/op              32 B/op          3 allocs/op
BenchmarkAtomic
BenchmarkAtomic              608           2003293 ns/op              24 B/op          2 allocs/op
PASS
ok      ben     3.239s
```

单个CPU情况下，每次操作内存分配， atomic 比 lock 少了1/3，但总的看差别不太大

[不同写法的性能差异](https://dashen.tech/2021/04/12/%E4%B8%8D%E5%90%8C%E5%86%99%E6%B3%95%E7%9A%84%E6%80%A7%E8%83%BD%E5%B7%AE%E5%BC%82/)

[Go测试](https://dashen.tech/2016/04/12/Go%E6%B5%8B%E8%AF%95/#%E5%9F%BA%E5%87%86%E6%B5%8B%E8%AF%95)


<br>


#### 两个CPU

<br>



`go test -bench=".*" -v -benchmem -cpu=2`

```go
goos: darwin
goarch: arm64
pkg: xxxxx
BenchmarkLock
BenchmarkLock-2              825           1363475 ns/op             441 B/op          3 allocs/op
BenchmarkAtomic
BenchmarkAtomic-2            831           1440089 ns/op              24 B/op          2 allocs/op
PASS
ok      ben     2.711s
```


相比单个CPU，每次操作耗时有所减少


<br>




#### 四个CPU

<br>

`go test -bench=".*" -v -benchmem -cpu=4`


```go
goos: darwin
goarch: arm64
pkg: xxxxx
BenchmarkLock
BenchmarkLock-4              567           2020170 ns/op             442 B/op          4 allocs/op
BenchmarkAtomic
BenchmarkAtomic-4            589           2011578 ns/op              33 B/op          2 allocs/op
PASS
ok      ben     2.841s
```


在arm架构的机器上，改变也不太大

<br>

#### 总结

<br>

从结果看，当使用多个CPU时，差距较为明显的是*分配内存次数*(如 4 allocs/op)和每次操作分配的内存大小(如 442B/op)。


总体来看使用 atomic 要比使用 lock 的性能要好

所以在只修改一个变量值的场景下，优先使用 atomic ，而不是 lock 锁


<br>

### 原理

<br>

> 原子操作一般是由 **硬件底层** 支持的，而锁则是由**操作系统层面来实现**的。比起使用锁，使用 CAS原子操作这个过程不会形成*临界区和创建临界区*，大大减少了同步对程序性能的影响，所以性能要高效一些。<br>
但原子操作也有一定的弊端，在被操作值频繁变更的情况下，很可能失败，需要一直重试直到成功为止，这种重试行为也可以理解为自旋spinning，长时间处于spinning将浪费CPU


[【os浅尝】实现自旋锁](https://www.bilibili.com/video/BV1Sf4y1s7Np/)


<br>



#### 原子操作

<br>

硬件层面实现。几乎所有的现代CPU指令都支持CAS的原子操作(X86下对应的是 CMPXCHG 汇编指令) 

有了这个原子操作，就可以用其来实现各种无锁（lock free）的数据结构,如最常见的 [无锁队列](https://blog.haohtml.com/archives/30670)


<br>


CAS 在Golang中是以共享内存的方式来实现的一种同步机制，它是一个原子操作，一般格式如下:

```go
func addValue(delta int32){
    for{
        oldValue := atomic.LoadInt32(&addr)
        if atomic.CompareAndSwapInt32(&addr, oldValue, oldValue+delta){
            break;
        }
    }
}
```

<br>


"先从一个内存地址 &addr 读取出来当前存储的值，假如读取完以后，没有其它线程对此变量 进行修改的话，则下面的 atomic.CompareAndSwapInt32 语句会在执行时先再判断一次它的值是否与上次的相等，这时必须是相等的，则直接更新它的值；如果在读取值后，有其它线程对变量值进行了修改，发现值不相等，这时就再重新开始下一轮的判断，直到修改成功为止。

对于 atomic.CompareAndSwapIntxx() 之类函数，其声明在[sync/atomic/doc.go]() ，实现在 [src/sync/atomic/asm.s](https://github.com/golang/go/blob/go1.15.6/src/sync/atomic/asm.s#L24-L37)，真正对应的汇编文件位于 [src/runtime/internal/atomic/*.s](https://github.com/golang/go/tree/go1.15.6/src/runtime/internal/atomic)，如64架构对应的文件为 [asm_amd64.s](https://github.com/golang/go/blob/go1.15.6/src/runtime/internal/atomic/asm_amd64.s)"


arm架构下atomic.CompareAndSwapInt64 和 atomic.CompareAndSwapUint64 对应的汇编是 [runtime∕internal∕atomic·Cas64]()；

```go
// bool runtime∕internal∕atomic·Cas64(uint64 *ptr, uint64 old, uint64 new)
// Atomically:
//      if(*val == *old){
//              *val = new;
//              return 1;
//      } else {
//              return 0;
//      }
TEXT runtime∕internal∕atomic·Cas64(SB), NOSPLIT, $0-25
	MOVD	ptr+0(FP), R0
	MOVD	old+8(FP), R1
	MOVD	new+16(FP), R2
again:
	LDAXR	(R0), R3
	CMP	R1, R3
	BNE	ok
	STLXR	R2, (R0), R3
	CBNZ	R3, again
ok:
	CSET	EQ, R0
	MOVB	R0, ret+24(FP)
	RET
```

<br>

#### 锁

<br>

可以保证一段代码的原子性，由操作系统来实现。锁的粒度越小越好



<br>


### 对比

<br>

> mutex 由操作系统实现，而 atomic 包中的原子操作则由底层硬件直接提供支持。在 CPU 实现的指令集里，有一些指令被封装进了 atomic 包，这些指令在执行的过程中是不允许中断（interrupt）的，因此原子操作可以在 lock-free 的情况下保证并发安全，并且它的性能也能做到随 CPU 个数的增多而线性扩展。<br>
若实现相同的功能，后者通常会更有效率，并且更能利用计算机多核的优势。所以，以后当我们想并发安全的更新一些变量的时候，我们应该优先选择用 atomic 来实现。

[到底啥是原子性--Mutex vs Atomic](https://ms2008.github.io/2019/05/12/golang-data-race/)



<br>


更多参考:

[sync/atomic - 原子操作](https://docs.kilvn.com/The-Golang-Standard-Library-by-Example/chapter16/16.02.html)

[Go是如何实现乐观锁（CAS理论）](https://zhuanlan.zhihu.com/p/159334753)