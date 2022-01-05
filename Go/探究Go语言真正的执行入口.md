---
title: 探究Go语言真正的执行入口
date: 2016-07-19 21:28:39
tags: Go
categories: runtime
---

> 编译好的可执行文件 真正的执行入口，并非我们所写的 main.main 函数，编译器总会插入一段引导代码，完成诸如命令行参数、运行时初始化等工作。然后才会进入用户逻辑。 (「Go语言学习笔记」)





**test.go:**

```go
package main

func main() {
	println("hello shuang!")
}
```

<br>

`go build -gcflags "-N -l" -o dashen test.go`


> 调试程序时，最好使用 -gcflags "-N -l"参数，关闭编译器代码优化和函数的内联，避免断点和单步执行无法准确对应源码行，避免小程序和局部变量被优化掉。(更多可参见[go build可选的那些参数](https://dashen.tech/2018/05/03/go-build%E5%8F%AF%E9%80%89%E7%9A%84%E9%82%A3%E4%BA%9B%E5%8F%82%E6%95%B0/),[Go中的内联优化](https://dashen.tech/2021/05/22/Go%E4%B8%AD%E7%9A%84%E5%86%85%E8%81%94%E4%BC%98%E5%8C%96/),[golang逃逸技术分析](https://dashen.tech/2021/05/24/golang%E9%80%83%E9%80%B8%E6%8A%80%E6%9C%AF%E5%88%86%E6%9E%90/))



<br>

使用gdb，在Linux机器上调试的具体过程详见 [gdb/lldb调试工具](https://dashen.tech/2019/10/30/%E7%A5%9E%E5%99%A8gdb/#%E6%8E%A2%E7%A9%B6Go-%E7%A8%8B%E5%BA%8F%E5%90%AF%E5%8A%A8%E8%BF%87%E7%A8%8B-%E5%8D%B3%E7%A1%AE%E5%AE%9A%E7%9C%9F%E6%AD%A3%E7%9A%84%E5%85%A5%E5%8F%A3%E5%87%BD%E6%95%B0)

<br>


在此使用dlv在Mac上进行调试 [使用dlv调试Go程序](https://dashen.tech/2021/04/02/%E4%BD%BF%E7%94%A8dlv%E8%B0%83%E8%AF%95Go%E7%A8%8B%E5%BA%8F/)：







