---
title: golang获取调用者的方法名及所在行数
date: 2018-05-18 21:35:34
tags: Go
---


```go
package main

import (
	"fmt"
	"runtime"
)
import "log"

func main() {
	test()
}

func test() {
	test2()
}

func test2() {

	pc, file, line, ok := runtime.Caller(2)
	log.Println(pc)
	log.Println(file)
	log.Println(line)
	log.Println(ok)
	f := runtime.FuncForPC(pc)
	log.Println(f.Name())

	fmt.Println("--------------------")

	pc, file, line, ok = runtime.Caller(0)
	log.Println(pc)
	log.Println(file)
	log.Println(line)
	log.Println(ok)
	f = runtime.FuncForPC(pc)
	log.Println(f.Name())

	fmt.Println("--------------------")

	pc, file, line, ok = runtime.Caller(1)
	log.Println(pc)
	log.Println(file)
	log.Println(line)
	log.Println(ok)
	f = runtime.FuncForPC(pc)
	log.Println(f.Name())
}
```

<br>

输出:

```go
2018/05/18 21:25:59 17426448
2018/05/18 21:25:59 /Users/dashen/go/src/note/b_demo/runtime123.go
2018/05/18 21:25:59 10
2018/05/18 21:25:59 true
2018/05/18 21:25:59 main.main
--------------------
2018/05/18 21:25:59 17427156
2018/05/18 21:25:59 /Users/dashen/go/src/note/b_demo/runtime123.go
2018/05/18 21:25:59 29
2018/05/18 21:25:59 true
2018/05/18 21:25:59 main.test2
--------------------
2018/05/18 21:25:59 17426447
2018/05/18 21:25:59 /Users/dashen/go/src/note/b_demo/runtime123.go
2018/05/18 21:25:59 14
2018/05/18 21:25:59 true
2018/05/18 21:25:59 main.test
```


<br>


相关代码:

*src/runtime/extern.go*

```go
// Caller reports file and line number information about function invocations on
// the calling goroutine's stack. The argument skip is the number of stack frames
// to ascend, with 0 identifying the caller of Caller.  (For historical reasons the
// meaning of skip differs between Caller and Callers.) The return values report the
// program counter, file name, and line number within the file of the corresponding
// call. The boolean ok is false if it was not possible to recover the information.
func Caller(skip int) (pc uintptr, file string, line int, ok bool) {
	rpc := make([]uintptr, 1)
	n := callers(skip+1, rpc[:])
	if n < 1 {
		return
	}
	frame, _ := CallersFrames(rpc).Next()
	return frame.PC, frame.File, frame.Line, frame.PC != 0
}
```