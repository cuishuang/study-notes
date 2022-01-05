---
title: Linux内核分析与应用6-系统调用
date: 2020-05-17 19:52:33
tags: Linux
---



<img src="Linux内核分析与应用6-系统调用/0.png" width = 100% height = 100% />


<br>


### 6.1 Linux中的各种API

<br>

LSB (Linux Standards Base)


<img src="Linux内核分析与应用6-系统调用/1.png" width = 100% height = 100% />


POSIX: 可移植操作系统接口(Portable Operating System Interface of UNIX)





Linux ABI:

(为了兼容)

<img src="Linux内核分析与应用6-系统调用/2.png" width = 100% height = 100% />


<br>


**内核API:**

主要是内核中标记为"EXPORT_SYMBOL"的函数

<br>

**思考:**

保持一个稳定的ABI和保持一个稳定的API相比,谁更困难,为什么?




---

 
<br>


### 6.2 系统调用机制

<br>


系统调用---内核的出口



各种工具:
<img src="Linux内核分析与应用6-系统调用/3.png" width = 100% height = 100% />

如 strace ls,就可以看到ls命令所调用的系统调用

<img src="Linux内核分析与应用6-系统调用/strace.png" width = 100% height = 100% />



<img src="Linux内核分析与应用6-系统调用/4.png" width = 100% height = 100% />


中断是异步的,异常是同步的,系统调用既可以是同步,也可以是异步

<img src="Linux内核分析与应用6-系统调用/5.png" width = 100% height = 100% />

系统调用号存放在eax寄存器中, 其实现所在的源文件也不在一起.

参数存放在寄存器中,一般参数不超过6个

<img src="Linux内核分析与应用6-系统调用/6.png" width = 100% height = 100% />


<img src="Linux内核分析与应用6-系统调用/7.png" width = 100% height = 100% />



参考:

[系统调用](http://wwww.kerneltravel.net/journal/iv/syscall.htm)


[Linux内核之旅-电子杂志](http://wwww.kerneltravel.net/?id=2)


---



 
<br>


### 6.3 动手实践-添加系统调用

<br>

**(系统调用的实例--日志收集系统)**








