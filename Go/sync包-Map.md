---
title: sync包-Map
date: 2010-03-02 23:49:38
tags: Go
categories: sync
---

<br>



sync.map 则是一种并发安全的 map，在 *Go 1.9* 引入



[Go 1.9 sync.Map揭秘](https://colobu.com/2017/07/11/dive-into-sync-Map/)


https://tonybai.com/2020/11/10/understand-sync-map-inside-through-examples/




标准库 sync.Map 支持并发读写 map，但写入性能较差,更适用于读多写少的场景

sync.map内部使用了读写分离的机制，即将耗时较长的写操作与耗时较短的读操作分离，放在俩个容器中进行，极大的提高了map的并发性能，且不用管理锁机制，使用起来更加便捷。(空间换时间的方式，使用 read 和 dirty 两个 map 来进行读写分离，降低锁时间来提高效率。
)


sync.Map的中有read和dirty俩个容器，其中写操作优先放在dirty中进行，读操作优先放在read中进行。

读写分离的优点在于：

执行读操作时是不用加锁的，写操作需要加锁
在dirty存在时，写操作都放在dirty中执行，不影响read中的读操作，因此当读的元素在read中时，是可以并发读写的


在对sync.Map的增、删、改、查操作时，都是先对read做操作，再对dirty操作。因此read中做标记可以有效减少对dirty的无效操作，nil表示dirty未创建，无需对dirty操作，expunged表示dirty创建成功，写操作可以放入dirty中执行。



[一口气搞懂 Go sync.map 所有知识点](https://mp.weixin.qq.com/s/8aufz1IzElaYR43ccuwMyA)


[亲儿子sync.Map，性能为何不如读写锁+Map？](https://mp.weixin.qq.com/s/GI29FXett_cRi-uHLeFGiA)


[深度解密Go语言之sync.map](https://mp.weixin.qq.com/s/mXOU8TElP8bbqaybRKN8eA)

[看过这篇剖析，你还不懂 Go sync.Map 吗？](https://mp.weixin.qq.com/s/kblDTqKlUaTITIppigq9yA)



@bcmills Do not you think it's very rediculous that Go has a built in func len for map while sync.Map does not have a corresponding function func (Map)Len() int to get its size?

<br>

No, I don't think it's "very [ridiculous]". Concurrent data structures are not the same as unsynchronized data structures. The built-in map type doesn't have (and doesn't need) a LoadOrStore, for example.

We should decide the API of each type based on its own tradeoffs. Consistency is a benefit, but there are costs to weigh it against.



[吐槽sync.Map为什么没有len方法的issue](https://github.com/golang/go/issues/20680)


[golang 并发安全Map以及分段锁的实现](https://segmentfault.com/a/1190000018448064)