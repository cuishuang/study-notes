---
title: Golang源码之syscall包
date: 2020-07-09 21:13:18
tags: Go
---


> go 版本为1.14, Mac电脑, 1.7 GHz 四核Intel Core i7

<br>

在`src/syscall/`路径下,打开`zerrors_darwin_amd64.go`文件,

<br>

第1245行:

```go
// Signals
const (
	SIGABRT   = Signal(0x6) //十进制数6
	SIGALRM   = Signal(0xe) //14
	SIGBUS    = Signal(0xa) //10
	SIGCHLD   = Signal(0x14) //20
	SIGCONT   = Signal(0x13) //19
	SIGEMT    = Signal(0x7) //7
	SIGFPE    = Signal(0x8) //8
	SIGHUP    = Signal(0x1) //1
	SIGILL    = Signal(0x4) //4
	SIGINFO   = Signal(0x1d) //29
	SIGINT    = Signal(0x2) //2
	SIGIO     = Signal(0x17) //23
	SIGIOT    = Signal(0x6) //6,为什么该值有两个?
	SIGKILL   = Signal(0x9) //9
	SIGPIPE   = Signal(0xd) //13
	SIGPROF   = Signal(0x1b) //27
	SIGQUIT   = Signal(0x3) //3
	SIGSEGV   = Signal(0xb) //11
	SIGSTOP   = Signal(0x11) //17
	SIGSYS    = Signal(0xc) //12
	SIGTERM   = Signal(0xf) //15
	SIGTRAP   = Signal(0x5) //5
	SIGTSTP   = Signal(0x12) //18
	SIGTTIN   = Signal(0x15) //21
	SIGTTOU   = Signal(0x16) //22
	SIGURG    = Signal(0x10) //16
	SIGUSR1   = Signal(0x1e) //30
	SIGUSR2   = Signal(0x1f) //31
	SIGVTALRM = Signal(0x1a) //26
	SIGWINCH  = Signal(0x1c) //28
	SIGXCPU   = Signal(0x18) //24
	SIGXFSZ   = Signal(0x19) //25
)
```


1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31, 共31个

*SIGIOT*和*SIGABRT*都是6,二者同义




参考:

[Linux进程间通信(IPC)的几种方式](http://www.dashen.tech/2020/06/16/Linux%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1-IPC-%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F/)


[linux kill -l 信号列表](https://www.cnblogs.com/xiao0913/p/11846212.html)


