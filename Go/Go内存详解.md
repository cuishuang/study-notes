---
title: Go内存详解
date: 2014-04-01 23:52:01
tags: [Go,Pearl]
categories: runtime
---


[C++ 堆区，栈区，数据段，bss段，代码区（详解）](https://blog.csdn.net/JACKSONMHLK/article/details/114392343)



<br>

## 1.内存分区

<br>

代码经过**预处理,编译,汇编,链接**4步后,生成一个可执行程序.(编译原理中的必学内容)

 <img src="Go内存详解/1.png" width = 100% height = 50% />

<br>

 <img src="Go内存详解/2.png" width = 100% height = 50% />

<br>

三大平台都有`size`命令,用来查看一个程序的信息


- text:表示代码区的大小.程序编译完之后,代码区的大小是固定的.

- data & objc(Windows中为bss): 程序中用到的数据内容 

- dec & hex:十进制大小和十六进制大小


<br>

在程序还没有运行之前,即还没有加载到内存前,其内部已经分好了三段信息,分别为:<font color="orange">代码区(text),(初始化)数据区(data),和未初始化数据区(bss).</font>

有时也会把data和bss合称为**静态区**或**全局区**

<br>

 <img src="Go内存详解/demo.jpeg" width = 100% height = 50% />

<br>

### 1.1 代码区(text)

<br>

存放CPU执行的机器指令.

1. 通常代码区是可共享的,即另外的执行程序可以调用它,使其可共享的目的是: 对于频繁被执行的程序,只需要在内存中有一份代码即可.

2. 代码区通常是**只读**的,使其只读的原因是防止程序意外地修改了它的指令.

3. 另外,代码区还规划了局部变量的相关信息.

<br>

### 1.2 全局初始化数据区/静态数据区(data)

<br>

该区包含了在程序中明确被初始化的全局变量,已经初始化的静态变量(包括全局静态变量和局部静态变量),以及常量数据(如字符串常量等)


<br>

### 1.3 未初始化数据区(bss)

<br>


Go中bss为0,因为Go语言所有的数据,默认都是初始化的,如定义了一个字符串,其默认为"",因此是在data区域.

而对于C语言,如声明了一个`int a`,是存在bss区域,而不是data区域`---`如果程序中有用到该变量,而又没有初始化,可能会导致错误的产生..

Go这一点与C不同,摒弃了放在bss区,而是放在了data区.


 <img src="Go内存详解/3.png" width = 100% height = 50% />


<br>

text区,data区和bss区,栈区(stack),堆区(heap),合称`内存四区`


 <img src="Go内存详解/4.png" width = 100% height = 50% />

  <img src="Go内存详解/5.png" width = 100% height = 50% />


  <br>

### 1.4 栈区(stack)

<br>


栈区大小是有限制的,大小为1M,

超过1M大小,就要放到堆区..

栈区大小限制可以扩展,Windows中最大可扩展为10M, Linux中最大可扩展为16M




<br>

### 1.5 堆区(heap)

<br>

堆区的大小和内存条的大小有关.一般堆区最大值不会超过内存大小的一半


 <img src="Go内存详解/6.png" width = 100% height = 50% />




 Go用new()创建一块空间,不需要管释放..因为有垃圾回收机制,去自动释放


 在Go中,对栈区和堆区集中管理,给用户即程序员暴露了一个虚拟的内存区域.

在32位系统上,这个地址一般以`0xc0420`开头

 <img src="Go内存详解/7.png" width = 100% height = 50% />


---

栈区是从**高地址向低地址**进行数据的存储



代码区和数据区是所有程序共享的吗?

代码区是的,数据区是独有的,B程序获取不到A的数据区(但可用通过一些手段/第三方软件,如Cheat Engine软件,可以用来修改其他程序的内存地址,如修改一些游戏里面参数的值)

 <img src="Go内存详解/8.png" width = 100% height = 50% />


<br>

---


<br>

## 2.Go Runtime内存分配

<br>


即在`内存四区`的基础上加了一层

 <img src="Go内存详解/9.png" width = 100% height = 50% />


可参考:

[TCMalloc原理](https://blog.csdn.net/u012122743/article/details/50679601)

<br>

### 2.1 基本策略

<br>

 Go启动后,内存消耗量会比较大,因为会申请**一大块内存**

1. 每次从操作系统申请一大块内存,以减少系统调用(并不一定是"大块连续内存")

2. 将内存切分成小块,构成双向链表

3. 为对象分配内存时,只需从大小合适的链表中提取一个小块即可

4. 回收对象内存时,将该小块内存重新归还到原链表,以便复用(而不是归还给操作系统,这样能减少系统调用)

5. 如果闲置内存太多,将尝试归还部分内存给操作系统,降低整体开销


*Go的内存操作模型`---`高端面试常问...*


<br>

**为什么用链表而不用数组?**

<br>

因为申请的空间,并不是一个连续的空间


 <img src="Go内存详解/10.png" width = 100% height = 50% />


  <img src="Go内存详解/11.png" width = 100% height = 50% />


<br>

### 2.2 内存管理单元

<br>


  <img src="Go内存详解/12.png" width = 100% height = 50% />

  _PageShift为上移或左移的意思<br>
  _PageSize为页大小

  将1左移13,即1后面加上13个0,这个二进制值即是8KB

  <img src="Go内存详解/13.png" width = 100% height = 50% />


- arean区域

- bitmap区域

- spans区域



  <img src="Go内存详解/14.png" width = 100% height = 50% />

  bitmap即 `位图`


  <img src="Go内存详解/15.png" width = 100% height = 50% />

用4个bit(位)来标住对象包含的指针和GC信息

垃圾回收时,先在bitmap中查找相关信息

---


  <br>

### 2.3 内存管理组件

<br>


 <img src="Go内存详解/16.png" width = 100% height = 50% />


内存分配由内存分配器完成,分配器由三种组件构成:


- **cache**: 每个运行期工作线程都会绑定一个cache,用于无锁object的分配



- **central**: 为所有的cache提供切分好的后辈span资源


- **heap**: 管理闲置span,需要时向操作系统申请内存

<br>

---

#### 2.3.1 cache

<br>


 <img src="Go内存详解/20.png" width = 100% height = 50% />

 <img src="Go内存详解/17.png" width = 100% height = 50% />

 <img src="Go内存详解/18.png" width = 100% height = 50% />
 <img src="Go内存详解/19.png" width = 100% height = 50% />



通过mcache,去管理mspan



span存的是页的信息,一页有8KB.



//runtime/sizeclasses.go
```go
const (
	_MaxSmallSize   = 32768
	smallSizeDiv    = 8
	smallSizeMax    = 1024
	largeSizeDiv    = 128
	_NumSizeClasses = 67
	_PageShift      = 13
)

var class_to_size = [_NumSizeClasses]uint16{0, 8, 16, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 896, 1024, 1152, 1280, 1408, 1536, 1792, 2048, 2304, 2688, 3072, 3200, 3456, 4096, 4864, 5376, 6144, 6528, 6784, 6912, 8192, 9472, 9728, 10240, 10880, 12288, 13568, 14336, 16384, 18432, 19072, 20480, 21760, 24576, 27264, 28672, 32768}
```


一个"页"分成了67个块内容,加起来正好是8KB大小.

而bitmap标识页里面的这些个块,到底有没使用


<br>

#### 2.3.2 central

<br>

//runtime/mcentral.go
 <img src="Go内存详解/21.png" width = 100% height = 50% />


 <img src="Go内存详解/22.png" width = 100% height = 50% />


<br>

---

<br>



#### 2.3.3 heap

<br>

三个层次,层层递进
 <img src="Go内存详解/23.png" width = 100% height = 50% />
 <img src="Go内存详解/24.png" width = 100% height = 50% />

//runtime/mheap.go

 <img src="Go内存详解/25.png" width = 100% height = 50% />
 <img src="Go内存详解/26.png" width = 100% height = 50% />


<br>


---

<br>

#### 三者关系


<br>


mheap负责管理申请堆区空间

`sizeclass`,译作"规格"


 <img src="Go内存详解/27.png" width = 100% height = 50% />


最接近数据块的是`mcache`,其每一个对象都绑定着相关信息,想使用时找`mcentral`,去找到一个对应规格的mspan.

由`mheap`去分配

---

  <br>

### 2.4 分配流程

<br>

 <img src="Go内存详解/28.png" width = 100% height = 50% />


 (一层层逐级向上找)

 <br>


- 计算待分配对象的规格(size_class)

- 从cache_alloc数组中找到规格相同的span

- 从span.manualFreeList链表主提取可用的object

- 如果span.manualFreeList为空,从central获取新的span

- 如果central.nonempty为空,从heap.free/freelarge获取,并切分成object链表

- 如果heap没有大小合适的span,向操作系统申请新的内存

<br>

### 2.5 释放流程

<br>

- 将标记为可回收的object交还给所属的span.freelist(在bitmap里面有标记)

- 该span被放回central,可以提供cache重新获取

- 如果span已全部回收object,将其交还给heap,以便重新分切复用

- 定期扫描heap里闲置的span,释放其占用的内存

注: 以上流程不包括大对象,它直接从heap分配和释放

<br>

### 2.6 总结

<br>

 <img src="Go内存详解/29.png" width = 100% height = 50% />

 Go语言的内存分配非常复杂,一个原则就是**能复用的一定要复用**

 - Go在程序启动时,会向操作系统申请一大块内存,之后自行管理

 - Go内存管理的基本单元是mspan,它由若干个**页**组成,每种mspan可以分配特定大小的object

 - mcache,mcentral,mheap是go内存管理的三大组件,层层递进. mcache管理线程在本地缓存的mspan; mcentral管理全局的mspan供所有线程使用; mheap管理Go的所有动态分配内存.

 - 一般小对象通过mspan分配内存; 大对象则直接由mheap分配内存


<br>

---

<br>


## 3. Go GC 垃圾回收

<br>


 <img src="Go内存详解/gc.png" width = 100% height = 50% />

Go的垃圾回收也是并行的



---

参考:

[带你领略Go源码的魅力 | Go内存原理详解](https://mp.weixin.qq.com/s/X_vf8U8CrmduVBRtMbctmg)



2021补充参考：


[可视化Go内存管理](https://tonybai.com/2020/03/10/visualizing-memory-management-in-golang/)

[图解Go运行时调度器](https://tonybai.com/2020/03/21/illustrated-tales-of-go-runtime-scheduler/)

已将Tony Bai大佬博客添加友链 ^_^


[Go memory allocator](https://www.processon.com/view/60edb61207912906d9feb748?fromnew=1)