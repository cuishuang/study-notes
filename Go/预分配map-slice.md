---
title: 预分配map/slice
date: 2018-12-15 23:06:52
tags: Go
---






参考自 [聊聊Go内存优化和相关底层机制](https://wudaijun.com/2019/09/go-performance-optimization/)


<b>

> 预分配主要针对map, slice这类数据结构，当知道它(大概)会有多少元素时，就提前分配，一来可以避免多次内存分配，另则也可以减少**space grow**带来的数据迁移(*map evacuate or slice copy*)开销

```go
package main

func main() {
}

func noPrepare() {
	n := 10
	src := make([]int, n)

	// 无预分配: 162 ns/op
	var dst []int

	for i := 0; i < n; i++ {
		// src[i]为默认值0
		dst = append(dst, src[i])
	}
}

func prePare() {
	n := 10
	src := make([]int, n)

	// 预分配，32.3 ns/op，提升了5倍
	dst2 := make([]int, 0, n)

	for i := 0; i < n; i++ {
		// src[i]为默认值0
		dst2 = append(dst2, src[i])
	}
}

func prePareAppend() {
	n := 10
	src := make([]int, n)

	dst2 := make([]int, 0, n)
	// 预分配+append...: 30.1 ns/op
	dst2 = append(dst2, src...)

}

func prePareCopy() {
	n := 10
	src := make([]int, n)

	dst2 := make([]int, 0, n)
	// 预分配+copy: 26.0 ns/op
	copy(dst2, src)
}
```

<br>

*webbench_test.go:*

```go
package main

import (
	"testing"
)

func BenchmarkTest1(b *testing.B) {
	for i := 0; i < b.N; i++ {
		noPrepare()
	}
}

func BenchmarkTest2(b *testing.B) {
	for i := 0; i < b.N; i++ {
		prePare()
	}
}

func BenchmarkTest3(b *testing.B) {
	for i := 0; i < b.N; i++ {
		prePareAppend()
	}
}

func BenchmarkTest4(b *testing.B) {
	for i := 0; i < b.N; i++ {
		prePareCopy()
	}
}
```


<br>

执行 `go test --bench=. -benchmem`


结果如下：


```go
goos: darwin
goarch: arm64
pkg: prepare
BenchmarkTest1-8         9454321               119.2 ns/op           328 B/op          6 allocs/op
BenchmarkTest2-8        32285764                36.72 ns/op          160 B/op          2 allocs/op
BenchmarkTest3-8        33689056                35.73 ns/op          160 B/op          2 allocs/op
BenchmarkTest4-8        36870369                32.37 ns/op          160 B/op          2 allocs/op
PASS
ok      prepare 5.033s
```

<br>

119.2 ns/op 表示每次操作耗时119.2纳秒
328 B 表示每次操作 用了328字节(Byte)
6 allocs 表示每次操作分配6次内存



<br>

很多代码分析工具如[prealloc](https://github.com/alexkohler/prealloc)可以帮助检查代码中可做的预分配优化