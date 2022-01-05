---
title: Go Race Detector
date: 2017-03-21 00:30:21
tags: [Go,Tools]
---



> 竞态: 无任何同步保护下,并行读写同一份数据(即 同一个变量。 读写指有写的同时还有其它读或者写，都是读则不算）


**多个goroutine 并发读写同一个变量，需要加锁，这应该是天经地义的常识！**

<br>

### 竞态检测

<br>

<font size=1 color="#00FFFF">

Go在1.1之后引入了竞争检测

可使用

go run -race xxx.go

go build -race xxx.go

go test -race xxx.go

来进行竞争检测

</font>


<br>


**demo.go:**
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	a := 222
	go func() {
		a = 666
	}()
	a = 888
	fmt.Println("a is ", a)

	time.Sleep(2 * time.Second)
}
```

<br>

**go func**触发的goroutine和**主goroutine**都可能对a进行修改


<br>

`go run -race demo.go:`

```go
a is  888
==================
WARNING: DATA RACE
Write at 0x00c0000ba008 by goroutine 7:
  main.main.func1()
      /Users/fliter/go/src/shuang/racee/demo.go:11 +0x28

Previous write at 0x00c0000ba008 by main goroutine:
  main.main()
      /Users/fliter/go/src/shuang/racee/demo.go:13 +0x70

Goroutine 7 (running) created at:
  main.main()
      /Users/fliter/go/src/shuang/racee/demo.go:10 +0x64
==================

Found 1 data race(s)
exit status 66
```
<br>

输出Warning: *goroutine7*运行到第11行和*main的goroutine*运行到第13行时触发竞争

且goroutine7是在第10行的时候产生的



<br>

<font size=1 color="#7FFFD4">


race的内部实现大概是同时开启多个goroutine执行同一个命令，且纪录每个变量的状态(值)

开启-race, 内存会增加5到10倍,执行时间增加2到20倍

所以不要在正式环境使用~

</font>





<br>


另一个demo：

```go
package main

import "fmt"

func main() {
	c := make(chan bool)
	m := make(map[string]string)
	go func() {
		m["1"] = "a" // First conflicting access.
		c <- true
	}()
	m["2"] = "b" // Second conflicting access.
	<-c
	for k, v := range m {
		fmt.Println(k, v)
	}
}
```


`go run -race demo2.go:`


```go
==================
WARNING: DATA RACE
Write at 0x00c0000a4180 by goroutine 7:
  runtime.mapaccess2_faststr()
      /usr/local/go/src/runtime/map_faststr.go:107 +0x48c
  main.main.func1()
      /Users/fliter/go/src/shuang/racee/demo2.go:9 +0x48

Previous write at 0x00c0000a4180 by main goroutine:
  runtime.mapaccess2_faststr()
      /usr/local/go/src/runtime/map_faststr.go:107 +0x48c
  main.main()
      /Users/fliter/go/src/shuang/racee/demo2.go:12 +0x9c

Goroutine 7 (running) created at:
  main.main()
      /Users/fliter/go/src/shuang/racee/demo2.go:8 +0x70
==================
1 a
2 b
Found 1 data race(s)
exit status 66
```



<br>


---


<br>

### 只有并发读，会不会有问题？

<br>


**并发读取，而没有写操作，是不会有问题的！**



`go run -race racee.go:`

```go
package main

func main() {
	aa := make(map[int]int)

	go func() {
		for {
			aa[0] = 5
			//_ = aa[2]
		}
	}()

	go func() {
		for {
			_ = aa[1]
		}
	}()

}
```

```go
==================
WARNING: DATA RACE
Read at 0x00c00009a000 by goroutine 7:
  runtime.evacuate_fast32()
      /usr/local/go/src/runtime/map_fast32.go:373 +0x37c
  main.main.func2()
      /Users/fliter/go/src/run/racee.go:15 +0x3c

Previous write at 0x00c00009a000 by goroutine 6:
  runtime.mapaccess1_fast64()
      /usr/local/go/src/runtime/map_fast64.go:12 +0x1ec
  main.main.func1()
      /Users/fliter/go/src/run/racee.go:8 +0x38

Goroutine 7 (running) created at:
  main.main()
      /Users/fliter/go/src/run/racee.go:13 +0x64

Goroutine 6 (running) created at:
  main.main()
      /Users/fliter/go/src/run/racee.go:6 +0x44
==================
fatal error: concurrent map read and map write
Found 1 data race(s)
exit status 66
```

<br>

而改为:

```go
package main

func main() {
	aa := make(map[int]int)

	go func() {
		for {
			//aa[0] = 5
			_ = aa[2]
		}
	}()

	go func() {
		for {
			_ = aa[1]
		}
	}()

}
```

则没有任何问题




<br>


参考：


[Go语言是如何实现race dectect的](https://www.zenlife.tk/race-dectect.md)


[谈谈 Golang 中的 Data Race](https://ms2008.github.io/2019/05/12/golang-data-race/)

[golang中的race检测](https://www.cnblogs.com/yjf512/p/5144211.html)


[golang工具race - 检测非法竞态访问数据](https://www.pengrl.com/p/41755/)
