---
title: 亲测体验Golang垃圾回收
date: 2020-11-20 19:12:39
tags: [Go,Pearl]
categories: runtime
---

<br>





## <font color="#8A2BE2">发展历程</font>




<br>


这部分是对[刘丹冰](https://github.com/aceld)大佬的[Golang中GC回收机制三色标记与混合写屏障](https://www.bilibili.com/video/BV1wz4y1y7Kd)学习的记录




<br>

尽可能在完成目标的情况下，缩短STW的时间(Stop the world时，业务代码需要暂停执行)

<br>

### <font color="#FF7F00">go 1.3 标记-清除算法</font>

<br>

最早的 *go 1.3*中的`标记-清除算法`，

后来将`清除`和`业务`并行处理，只在`标记`阶段STW


<img src="亲测体验Golang垃圾回收/0.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/1.png" width = 100% height = 100% />

<img src="亲测体验Golang垃圾回收/2.png" width = 100% height = 100% />



<br>


但在`标记`阶段，依然需要stw。于是 尝试采用新的标记模式，来替代mark and sweep标记法


<br>

### <font color="#FF7F00">go 1.5 三色标记法</font>

<br>

三个集合：白色标记表，灰色标记表，黑色标记表


每次只遍历一层。动态逐层遍历，而不像`标记-清除算法`一次性遍历


循环遍历，知道没有灰色节点

灰色只是一种临时状态，最终要么是黑的，要么是白的

<img src="亲测体验Golang垃圾回收/3.png" width = 100% height = 100% />


白色就是要收集的垃圾。


<br>

比之前版本的`标记-清除算法`要复杂一点。但是在遍历标记时，就不用stw了？？可以并行的标记和回收对象？


<br>

#### <font color="#008B8B">如果三色标记没有stw，是否会有问题？</font>

<br>

假如没有stw，业务代码和GC并行进行，那在标记过程中，如果GC从*对象2*层刚要扫*对象3*，此时(业务代码)*对象4*有一个指针q也指向了*对象3*，巧的是，这时(业务代码中的操作)*对象2*和*对象3*的引用关系消失。

GC继续执行，



<img src="亲测体验Golang垃圾回收/4.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/5.png" width = 100% height = 100% />





<img src="亲测体验Golang垃圾回收/6.png" width = 100% height = 100% />




<img src="亲测体验Golang垃圾回收/7.png" width = 100% height = 100% />


所以不stw的话，会有问题。如果二者同时出现，本来不是垃圾，结果被回收了(*对象3*)

<img src="亲测体验Golang垃圾回收/8.png" width = 100% height = 100% />


1. 存在隔代引用(爷爷B引用爷爷A的孙子)
2. 儿子和孙子之间关系消失(A的儿子和孙子关系断裂)




<img src="亲测体验Golang垃圾回收/9.png" width = 100% height = 100% />



<br>

#### <font color="#008B8B">强弱三色不变式</font>

<br>


<img src="亲测体验Golang垃圾回收/10.png" width = 100% height = 100% />

<br>


**强三色不变式:** 强制性的不允许*黑色*对象引用*白色*对象


<img src="亲测体验Golang垃圾回收/11.png" width = 100% height = 100% />

<br>


**弱三色不变式:** 当白色对象存在其他灰色对象对它的引用，或可达它的链路上游存在灰色对象时，这种情况下黑色对象可以引用白色对象  (黑色可以直接引用白色，但要保证白色的上游有灰色对象`---`因为这样白色对象就可以被保护，而不会被当成垃圾)




<img src="亲测体验Golang垃圾回收/12.png" width = 100% height = 100% />


<br>

<img src="亲测体验Golang垃圾回收/13.png" width = 100% height = 100% />


<br>


<br>

#### <font color="#008B8B">屏障机制</font>

<br>

<img src="亲测体验Golang垃圾回收/14.png" width = 100% height = 100% />


<br>


**插入屏障：**


<img src="亲测体验Golang垃圾回收/15.png" width = 100% height = 100% />



*为保证栈的性能，插入屏障不在栈上使用*


<font size=1 color="grey">

&nbsp;&nbsp;GC 垃圾回收不是只针对堆吗?为什么栈还需要三色标记呢?


> 每个调度器的结构体有两个G，一个是crug，代表结构体M当前绑定的结构体G。另一个是G0，是带有调度栈的goroutine，这是一个比较特殊的goroutine。 注意： 普通的goroutine的栈是在堆上分配的可增长的栈。  而G0的栈是M对应的线程的栈。 所有调度相关的代码，会先切换到该goroutine栈中再执行。也就是说线程的栈也是用的g实现，而不是使用os的。



</font>
<img src="亲测体验Golang垃圾回收/16.png" width = 100% height = 100% />



<img src="亲测体验Golang垃圾回收/17.png" width = 100% height = 100% />



如何保证栈上的变量不被误清除？

在准备回收白色前，重新遍历扫描(rescan)一次栈空间，此时要加STW保护..

和之前比好多了。。但依然有stw


<img src="亲测体验Golang垃圾回收/18.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/19.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/20.png" width = 100% height = 100% />





<br>

**删除屏障：**


删除也是写操作，只不过写的内容为nil



<img src="亲测体验Golang垃圾回收/21.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/22.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/23.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/24.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/25.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/26.png" width = 100% height = 100% />



对象5，2，3在这里其实都没有被引用，其实都是垃圾，但没有被(这次GC)回收



<img src="亲测体验Golang垃圾回收/27.png" width = 100% height = 100% />


另外，删除写屏障在启动前，也会启用stw，把当前的快照进行记录。为了清除时比较。


<br>

### <font color="#FF7F00">go 1.8及之后 混合写屏障</font>

<br>


即把 *插入写屏障*和*删除写屏障*结合，且多了一个栈默认变为黑色的操作


<img src="亲测体验Golang垃圾回收/28.png" width = 100% height = 100% />

<img src="亲测体验Golang垃圾回收/29.png" width = 100% height = 100% />



为了优化掉rescan，一开始优先扫描栈并标记为黑色，GC期间新创建的栈上的变量也标记为黑色。

而且为保证性能，栈上依然没有任何屏障。

堆上启用混合写屏障





<img src="亲测体验Golang垃圾回收/30.png" width = 100% height = 100% />

<img src="亲测体验Golang垃圾回收/31.png" width = 100% height = 100% />




<br>

#### 混合写屏障场景1：对象被一个堆对象删除引用，成为栈对象的下游

<br>



<img src="亲测体验Golang垃圾回收/hybird0.png" width = 100% height = 100% />



<img src="亲测体验Golang垃圾回收/hybird1.png" width = 100% height = 100% />



<img src="亲测体验Golang垃圾回收/h2.png" width = 100% height = 100% />



<img src="亲测体验Golang垃圾回收/h3.png" width = 100% height = 100% />



<br>

#### 混合写屏障场景2：对象被一个栈对象删除引用，成为另一个栈对象的下游

<br>



<img src="亲测体验Golang垃圾回收/h4.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/h5.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/h6.png" width = 100% height = 100% />


<img src="亲测体验Golang垃圾回收/h7.png" width = 100% height = 100% />



在栈上延迟一轮被回收，代价比堆上的延迟一轮回收的代价要小得多~因为栈上的变量一般都比较小，不太会有很大的对象


<br>

#### 混合写屏障场景3：对象被一个堆对象删除引用，成为另一个堆对象的下游

<br>



<img src="亲测体验Golang垃圾回收/h8.png" width = 100% height = 100% />

<img src="亲测体验Golang垃圾回收/h9.png" width = 100% height = 100% />

<img src="亲测体验Golang垃圾回收/h10.png" width = 100% height = 100% />

<img src="亲测体验Golang垃圾回收/h11.png" width = 100% height = 100% />


<br>

#### 混合写屏障场景4：对象被一个栈对象删除引用，成为另一个堆对象的下游

<br>

<img src="亲测体验Golang垃圾回收/h12.png" width = 100% height = 100% />



<img src="亲测体验Golang垃圾回收/h13.png" width = 100% height = 100% />




<img src="亲测体验Golang垃圾回收/h14.png" width = 100% height = 100% />



<img src="亲测体验Golang垃圾回收/h15.png" width = 100% height = 100% />



<br>

---

<br>


更多资料：

[《Golang修养之路》](https://www.kancloud.cn/aceld/golang/1958308)


[思维导图](https://pan.baidu.com/disk/pdfview?path=%2F%E7%AC%AC%E5%9B%9B%E8%AE%B2%EF%BC%9AGolang%E4%B8%89%E8%89%B2%E6%A0%87%E8%AE%B0%2B%E6%B7%B7%E5%90%88%E5%86%99%E5%B1%8F%E9%9A%9C%E6%9C%BA%E5%88%B6.pdf&size=2006043&fsid=708190894892200)





<br>

---

<br>



## <font color="#8A2BE2">GC触发时机</font>



<img src="亲测体验Golang垃圾回收/time.png" width = 100% height = 100% />


- 分配内存时，达到GOGC比例 &nbsp; &nbsp;{分配内存时, 当前已分配内存与上一次GC 结束时存活对象的内存达到某个比例时就触发GC。（默认配置会在堆内存达到上一次垃圾收集的 2 倍时，触发新一轮的垃圾收集，可以通过环境变量 GOGC 调整，在默认情况下它的值为 100，即增长 100% 的堆内存才会触发 GC。}


- 后台触发`---`sysmon检测 &nbsp; &nbsp;{ {sysmon 检测出一段时间内（由 runtime.forcegcperiod 变量控制，默认为 2 分钟）没有触发过 GC，就会触发新的 GC。}


- 手动触发`---`runtime.GC &nbsp; &nbsp;{调用 runtime.GC() 强制触发GC }

<br>

[Go GC](https://www.processon.com/view/60fbe47a637689719d24c16b?fromnew=1)





<br>

---

<br>



## <font color="#8A2BE2">亲测体验</font>


[GC 的认识](https://github.com/qcrao/Go-Questions/blob/master/GC/GC.md)