---
title: Go unsafe包
date: 2017-03-22 21:55:08
tags: Go
---




[src/unsafe](https://golang.org/src/unsafe/unsafe.go)路径下只有*unsafe.go*一个文件:


<details>

<summary><b>unsafe.go:</b></summary>

```go
// Copyright 2009 The Go Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.

/*
	Package unsafe contains operations that step around the type safety of Go programs.

	Packages that import unsafe may be non-portable and are not protected by the
	Go 1 compatibility guidelines.
*/
package unsafe

// ArbitraryType is here for the purposes of documentation only and is not actually
// part of the unsafe package. It represents the type of an arbitrary Go expression.
type ArbitraryType int

// Pointer represents a pointer to an arbitrary type. There are four special operations
// available for type Pointer that are not available for other types:
//	- A pointer value of any type can be converted to a Pointer.
//	- A Pointer can be converted to a pointer value of any type.
//	- A uintptr can be converted to a Pointer.
//	- A Pointer can be converted to a uintptr.
// Pointer therefore allows a program to defeat the type system and read and write
// arbitrary memory. It should be used with extreme care.
//
// The following patterns involving Pointer are valid.
// Code not using these patterns is likely to be invalid today
// or to become invalid in the future.
// Even the valid patterns below come with important caveats.
//
// Running "go vet" can help find uses of Pointer that do not conform to these patterns,
// but silence from "go vet" is not a guarantee that the code is valid.
//
// (1) Conversion of a *T1 to Pointer to *T2.
//
// Provided that T2 is no larger than T1 and that the two share an equivalent
// memory layout, this conversion allows reinterpreting data of one type as
// data of another type. An example is the implementation of
// math.Float64bits:
//
//	func Float64bits(f float64) uint64 {
//		return *(*uint64)(unsafe.Pointer(&f))
//	}
//
// (2) Conversion of a Pointer to a uintptr (but not back to Pointer).
//
// Converting a Pointer to a uintptr produces the memory address of the value
// pointed at, as an integer. The usual use for such a uintptr is to print it.
//
// Conversion of a uintptr back to Pointer is not valid in general.
//
// A uintptr is an integer, not a reference.
// Converting a Pointer to a uintptr creates an integer value
// with no pointer semantics.
// Even if a uintptr holds the address of some object,
// the garbage collector will not update that uintptr's value
// if the object moves, nor will that uintptr keep the object
// from being reclaimed.
//
// The remaining patterns enumerate the only valid conversions
// from uintptr to Pointer.
//
// (3) Conversion of a Pointer to a uintptr and back, with arithmetic.
//
// If p points into an allocated object, it can be advanced through the object
// by conversion to uintptr, addition of an offset, and conversion back to Pointer.
//
//	p = unsafe.Pointer(uintptr(p) + offset)
//
// The most common use of this pattern is to access fields in a struct
// or elements of an array:
//
//	// equivalent to f := unsafe.Pointer(&s.f)
//	f := unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.f))
//
//	// equivalent to e := unsafe.Pointer(&x[i])
//	e := unsafe.Pointer(uintptr(unsafe.Pointer(&x[0])) + i*unsafe.Sizeof(x[0]))
//
// It is valid both to add and to subtract offsets from a pointer in this way.
// It is also valid to use &^ to round pointers, usually for alignment.
// In all cases, the result must continue to point into the original allocated object.
//
// Unlike in C, it is not valid to advance a pointer just beyond the end of
// its original allocation:
//
//	// INVALID: end points outside allocated space.
//	var s thing
//	end = unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Sizeof(s))
//
//	// INVALID: end points outside allocated space.
//	b := make([]byte, n)
//	end = unsafe.Pointer(uintptr(unsafe.Pointer(&b[0])) + uintptr(n))
//
// Note that both conversions must appear in the same expression, with only
// the intervening arithmetic between them:
//
//	// INVALID: uintptr cannot be stored in variable
//	// before conversion back to Pointer.
//	u := uintptr(p)
//	p = unsafe.Pointer(u + offset)
//
// Note that the pointer must point into an allocated object, so it may not be nil.
//
//	// INVALID: conversion of nil pointer
//	u := unsafe.Pointer(nil)
//	p := unsafe.Pointer(uintptr(u) + offset)
//
// (4) Conversion of a Pointer to a uintptr when calling syscall.Syscall.
//
// The Syscall functions in package syscall pass their uintptr arguments directly
// to the operating system, which then may, depending on the details of the call,
// reinterpret some of them as pointers.
// That is, the system call implementation is implicitly converting certain arguments
// back from uintptr to pointer.
//
// If a pointer argument must be converted to uintptr for use as an argument,
// that conversion must appear in the call expression itself:
//
//	syscall.Syscall(SYS_READ, uintptr(fd), uintptr(unsafe.Pointer(p)), uintptr(n))
//
// The compiler handles a Pointer converted to a uintptr in the argument list of
// a call to a function implemented in assembly by arranging that the referenced
// allocated object, if any, is retained and not moved until the call completes,
// even though from the types alone it would appear that the object is no longer
// needed during the call.
//
// For the compiler to recognize this pattern,
// the conversion must appear in the argument list:
//
//	// INVALID: uintptr cannot be stored in variable
//	// before implicit conversion back to Pointer during system call.
//	u := uintptr(unsafe.Pointer(p))
//	syscall.Syscall(SYS_READ, uintptr(fd), u, uintptr(n))
//
// (5) Conversion of the result of reflect.Value.Pointer or reflect.Value.UnsafeAddr
// from uintptr to Pointer.
//
// Package reflect's Value methods named Pointer and UnsafeAddr return type uintptr
// instead of unsafe.Pointer to keep callers from changing the result to an arbitrary
// type without first importing "unsafe". However, this means that the result is
// fragile and must be converted to Pointer immediately after making the call,
// in the same expression:
//
//	p := (*int)(unsafe.Pointer(reflect.ValueOf(new(int)).Pointer()))
//
// As in the cases above, it is invalid to store the result before the conversion:
//
//	// INVALID: uintptr cannot be stored in variable
//	// before conversion back to Pointer.
//	u := reflect.ValueOf(new(int)).Pointer()
//	p := (*int)(unsafe.Pointer(u))
//
// (6) Conversion of a reflect.SliceHeader or reflect.StringHeader Data field to or from Pointer.
//
// As in the previous case, the reflect data structures SliceHeader and StringHeader
// declare the field Data as a uintptr to keep callers from changing the result to
// an arbitrary type without first importing "unsafe". However, this means that
// SliceHeader and StringHeader are only valid when interpreting the content
// of an actual slice or string value.
//
//	var s string
//	hdr := (*reflect.StringHeader)(unsafe.Pointer(&s)) // case 1
//	hdr.Data = uintptr(unsafe.Pointer(p))              // case 6 (this case)
//	hdr.Len = n
//
// In this usage hdr.Data is really an alternate way to refer to the underlying
// pointer in the string header, not a uintptr variable itself.
//
// In general, reflect.SliceHeader and reflect.StringHeader should be used
// only as *reflect.SliceHeader and *reflect.StringHeader pointing at actual
// slices or strings, never as plain structs.
// A program should not declare or allocate variables of these struct types.
//
//	// INVALID: a directly-declared header will not hold Data as a reference.
//	var hdr reflect.StringHeader
//	hdr.Data = uintptr(unsafe.Pointer(p))
//	hdr.Len = n
//	s := *(*string)(unsafe.Pointer(&hdr)) // p possibly already lost
//
type Pointer *ArbitraryType

// Sizeof takes an expression x of any type and returns the size in bytes
// of a hypothetical variable v as if v was declared via var v = x.
// The size does not include any memory possibly referenced by x.
// For instance, if x is a slice, Sizeof returns the size of the slice
// descriptor, not the size of the memory referenced by the slice.
// The return value of Sizeof is a Go constant.
func Sizeof(x ArbitraryType) uintptr

// Offsetof returns the offset within the struct of the field represented by x,
// which must be of the form structValue.field. In other words, it returns the
// number of bytes between the start of the struct and the start of the field.
// The return value of Offsetof is a Go constant.
func Offsetof(x ArbitraryType) uintptr

// Alignof takes an expression x of any type and returns the required alignment
// of a hypothetical variable v as if v was declared via var v = x.
// It is the largest value m such that the address of v is always zero mod m.
// It is the same as the value returned by reflect.TypeOf(x).Align().
// As a special case, if a variable s is of struct type and f is a field
// within that struct, then Alignof(s.f) will return the required alignment
// of a field of that type within a struct. This case is the same as the
// value returned by reflect.TypeOf(s.f).FieldAlign().
// The return value of Alignof is a Go constant.
func Alignof(x ArbitraryType) uintptr
```
</detail>

<br>


只有函数的签名和类型定义，但没有实现的代码：无论是 Go([go:linkname方式]()) 还是汇编的代码(如[byteag]()里面的xx就是汇编代码实现)都没有。之所以如此，是因为 unsafe 包的功能需要在层次更低的编译器层面实现，所以这个包其实是内置在编译器里面实现的，*unsafe.go*这个文件只是为了达到文档记录的目的。

<br>


将注释移除,其实只有以下这几个函数和类型

```go
package unsafe

func Alignof(x ArbitraryType) uintptr
func Offsetof(x ArbitraryType) uintptr
func Sizeof(x ArbitraryType) uintptr
type ArbitraryType
type Pointer
```



<br>

其中 `ArbitraryType` 类型只是为了文档记录的目的而存在，实际并没有参与到 unsafe 包的实现。这个类型代表了任意的 Go 语言表达式。

所以实际上 unsafe 包就只包含三个函数和一个类型

<br>





### func Sizeof(x ArbitraryType) uintptr




<br>

```go
package main

import (
	"fmt"
	"unsafe"
)

type X struct {
	n1 int16
	n2 int16
}

func main() {

	fmt.Println(unsafe.Sizeof(X{}))
}
```

输出: 4


<br>

> X 结构体有两个字段，其中每一个都占 2 个字节，所以整个结构体占用 size(n1) + size(n2) + size(X) = 2 + 2 + 0 = 4


<br>


### func Offsetof(x ArbitraryType) uintptr

<br>


返回的是 offset（偏移值）


```go
package main

import (
	"fmt"
	"unsafe"
)

type X struct {
	n1 int16
	n2 int16
}

func main() {

	fmt.Println(unsafe.Offsetof(X{}.n1))
	fmt.Println(unsafe.Offsetof(X{}.n2))
}
```

输出:  0 2


<br>


### func Alignof(x ArbitraryType) uintptr

<br>


[各种数据类型在内存中的结构](https://dashen.tech/2020/06/27/%E5%8F%8D%E5%B0%84/#%E5%90%84%E7%A7%8D%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%9C%A8%E5%86%85%E5%AD%98%E4%B8%AD%E7%9A%84%E7%BB%93%E6%9E%84)


[有关内存对齐](https://dashen.tech/2020/02/03/%E6%9C%89%E5%85%B3%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90/)



<br>

```go
package main

import (
	"fmt"
	"unsafe"
)

type X struct {
	n1 int8
	n2 int16
}

func main() {

	fmt.Println(unsafe.Sizeof(X{}))
}

```


输出 4

<br>

为什么不是3?


> 由于 alignment 机制的要求，n2 的内存起始地址应该是自身大小的整数倍，也就是说它的起始地址只能是 0、2、4、6、8 等偶数，所以 n2 的起始地址没有紧接着 n1 后面，而是空出了 1 个字节。最后导致结构体 X 的大小是 4 而不是 3。机智的读者可能会想到：n1 和 n2 换个位置会怎样呢？这样一来，n2 的起始地址是 0，而 n1 的其实地址是 2，这么一来结构体 X 的大小就变成 3 了吧？答案是……不对的。原因还是因为 alignment，因为 alignment 除了要求字段的其实地址应该是自身大小的整数倍，还要求整个结构体的大小，是结构体中最大的字段的大小的整数倍，这使得结构体可以由多个内存块组成，其中每个内存块的大小都等于最大的字段的大小。可以利用这个tips来减少结构体的内存占用




<br>



```go
package main

import (
	"fmt"
	"unsafe"
)

type First struct {
	a int8
	b int64
	c int8
}

type Second struct {
	a int8
	c int8
	b int64
}

func main() {
	fmt.Println(unsafe.Sizeof(First{}), unsafe.Sizeof(Second{}))

}
```

输出为: 24  16

> 上面两个结构体大小不同，是因为 *First* 结构体由三个大小为 8 字节的内存块组成：**Sizeof(First.a) + 7 个空闲的字节 + Sizeof(First.b) + Sizeof(First.c) + 7 个空闲的字节 = 24 字节**;  而 *Second* 结构体只包含  2 个 大小为 8 字节的内存块：**Sizeof(Second.a) + Sizeof(Second.b) + 6 个空闲的字节 + Sizeof(Second.b) = 16 字节**。下次定义结构体的时候可以用上这个小知识(让占空间大的类型尽可能在后面)



<br>



### 总结这三个函数的用法

<br>


```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	
	fmt.Println("Size of x: ", unsafe.Sizeof(x))
	fmt.Println("Size of x.c: ", unsafe.Sizeof(x.c))

	fmt.Println("Alignment of x.a: ", unsafe.Alignof(x.a))
	fmt.Println("Alignment of x.b: ", unsafe.Alignof(x.b))
	fmt.Println("Alignment of x.c: ", unsafe.Alignof(x.c))

	fmt.Println("\nOffset of x.a: ", unsafe.Offsetof(x.a))
	fmt.Println("Offset of x.b: ", unsafe.Offsetof(x.b))
	fmt.Println("Offset of x.c: ", unsafe.Offsetof(x.c))

}

var x struct {
	a int64
	b bool
	c string
}
```


输出为:

```go
Size of x:  32
Size of x.c:  16
Alignment of x.a:  8
Alignment of x.b:  1
Alignment of x.c:  8

Offset of x.a:  0
Offset of x.b:  8
Offset of x.c:  16

```


<br>



这三个方法都是在[编译期]()执行的，这意味着只要编译器没有报错，在运行时也不会有问题发生

而 `unsafe.Pointer`则不然,有可能会发生[运行时错误]()



<br>



### type Pointer


<br>


[Golang 语言中的非类型安全指针](https://mp.weixin.qq.com/s/MvULt7x0m4IBmz1bNzLvCQ)



---


<br>


参考:


[灵魂一问：unsafe 包真的不安全吗？](https://mp.weixin.qq.com/s/PtAWvFrEUC8qXT3b46F7Rg)



