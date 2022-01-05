---
title: runtime.Caller的性能问题
date: 2019-11-11 23:32:07
tags: Go
categories: runtime
---


前置篇 [golang获取调用者的方法名及所在行数](https://dashen.tech/2018/05/18/golang%E8%8E%B7%E5%8F%96%E8%B0%83%E7%94%A8%E8%80%85%E7%9A%84%E6%96%B9%E6%B3%95%E5%90%8D%E5%8F%8A%E6%89%80%E5%9C%A8%E8%A1%8C%E6%95%B0/)



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

<font size=1>
Caller报告有关函数调用的文件和行号信息
调用 goroutine 的堆栈。 参数 skip 是堆栈帧的数量
上升，0 标识呼叫者的caller。 （由于历史原因
  跳过的含义在调用者和调用者之间有所不同。）返回值报告
  相应的文件中的程序计数器、文件名和行号
称呼。 如果无法恢复信息，则布尔值 ok 为 false。
</font>

<br>



- skip： 堆栈帧数，0 为当前函数，1为上一层函数


- pcuintptr这个返回的是函数指针

- file是函数所在文件名目录

- line所在行号

- ok 是否可以获取到信息

<br>


```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	test()
}

func test() {

	fmt.Printf("当前函数的文件/行号等信息：")
	fmt.Println(runtime.Caller(0))

	fmt.Printf("上一层函数的文件/行号等信息：")
	fmt.Println(runtime.Caller(1))

	fmt.Printf("上两层函数的文件/行号等信息：")
	fmt.Println(runtime.Caller(2))

	fmt.Printf("上三层函数的文件/行号等信息：")
	fmt.Println(runtime.Caller(3))

	fmt.Printf("上四层函数的文件/行号等信息：")
	fmt.Println(runtime.Caller(4))
}
```

执行结果：

```go
当前函数的文件/行号等信息：4297144295 /Users/fliter/go/src/shuang/runtimee/call.go 15 true
上一层函数的文件/行号等信息：4297144159 /Users/fliter/go/src/shuang/runtimee/call.go 9 true
上两层函数的文件/行号等信息：4296724555 /usr/local/go/src/runtime/proc.go 225 true
上三层函数的文件/行号等信息：4296909139 /usr/local/go/src/runtime/asm_arm64.s 1130 true
上四层函数的文件/行号等信息：0  0 false
```


<br>

Next在不停地迭代

```go
// Next returns frame information for the next caller.
// If more is false, there are no more callers (the Frame value is valid).
func (ci *Frames) Next() (frame Frame, more bool) {
	for len(ci.frames) < 2 {
		// Find the next frame.
		// We need to look for 2 frames so we know what
		// to return for the "more" result.
		if len(ci.callers) == 0 {
			break
		}
		pc := ci.callers[0]
		ci.callers = ci.callers[1:]
		funcInfo := findfunc(pc)
		if !funcInfo.valid() {
			if cgoSymbolizer != nil {
				// Pre-expand cgo frames. We could do this
				// incrementally, too, but there's no way to
				// avoid allocation in this case anyway.
				ci.frames = append(ci.frames, expandCgoFrames(pc)...)
			}
			continue
		}
		f := funcInfo._Func()
		entry := f.Entry()
		if pc > entry {
			// We store the pc of the start of the instruction following
			// the instruction in question (the call or the inline mark).
			// This is done for historical reasons, and to make FuncForPC
			// work correctly for entries in the result of runtime.Callers.
			pc--
		}
		name := funcname(funcInfo)
		if inldata := funcdata(funcInfo, _FUNCDATA_InlTree); inldata != nil {
			inltree := (*[1 << 20]inlinedCall)(inldata)
			// Non-strict as cgoTraceback may have added bogus PCs
			// with a valid funcInfo but invalid PCDATA.
			ix := pcdatavalue1(funcInfo, _PCDATA_InlTreeIndex, pc, nil, false)
			if ix >= 0 {
				// Note: entry is not modified. It always refers to a real frame, not an inlined one.
				f = nil
				name = funcnameFromNameoff(funcInfo, inltree[ix].func_)
				// File/line is already correct.
				// TODO: remove file/line from InlinedCall?
			}
		}
		ci.frames = append(ci.frames, Frame{
			PC:       pc,
			Func:     f,
			Function: name,
			Entry:    entry,
			funcInfo: funcInfo,
			// Note: File,Line set below
		})
	}

	// Pop one frame from the frame list. Keep the rest.
	// Avoid allocation in the common case, which is 1 or 2 frames.
	switch len(ci.frames) {
	case 0: // In the rare case when there are no frames at all, we return Frame{}.
		return
	case 1:
		frame = ci.frames[0]
		ci.frames = ci.frameStore[:0]
	case 2:
		frame = ci.frames[0]
		ci.frameStore[0] = ci.frames[1]
		ci.frames = ci.frameStore[:1]
	default:
		frame = ci.frames[0]
		ci.frames = ci.frames[1:]
	}
	more = len(ci.frames) > 0
	if frame.funcInfo.valid() {
		// Compute file/line just before we need to return it,
		// as it can be expensive. This avoids computing file/line
		// for the Frame we find but don't return. See issue 32093.
		file, line := funcline1(frame.funcInfo, frame.PC, false)
		frame.File, frame.Line = file, int(line)
	}
	return
}
```



使用`runtime.Caller`能够拿到当前执行的文件名和行号，这个方法几乎在所有的日志组件里都有使用

单次消耗的时间可以忽略不计，但对于日志量巨大的服务而言影响还是很大的


<br>

比较好的做法是，不是所有日志都调用该方法，而是分级别，或让用户使用hook来决定哪条日志需要打印出文件名和行号信息



<br>




---

<br>


参考：

[golang日志组件使用runtime.Caller性能问题分析](https://cloud.tencent.com/developer/article/1385947)

[鸟窝-如何在Go的函数中得到调用者函数名?](https://colobu.com/2018/11/03/get-function-name-in-go/)