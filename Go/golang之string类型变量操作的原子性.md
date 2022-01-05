---
title: golang之string类型变量操作的原子性
date: 2018-07-19 21:22:20
tags: [Go,Pearl]
---




在并发场景下，string跟interface一样，都是需要使用atomic包来保证读写的原子性。


<br>

```go
package main

import (
	"fmt"
	"time"
)

var a = "0"

func main() {

	ch := make(chan string)

	go func() {
		i := 1

		for {

			if i%2 == 0 {
				a = "0"
			} else {
				a = "aa"
			}
			time.Sleep(1 * time.Millisecond)
			i++
		}
	}()

	go func() {
		for {
			b := a
			if b != "0" && b != "aa" {
				ch <- b
			}
		}
	}()

	for i := 0; i < 10; i++ {
		fmt.Println("Got strange string: ", <-ch)
	}

}

```

<br>

按预期, `ch` 不可能被写入,因为 `b` 的值只可能是 "0" 或 "aa".

但实际运行:


(即两个协程,一个不停对`a`重新赋值,另一个选出`a`的值不为"0"或"aa"的,存入一个channel,并打印. 当有10次不为"0"或"aa"后,结束打印,程序运行完成)

```rs
Got strange string:  05
Got strange string:  a
Got strange string:  a
Got strange string:  05
Got strange string:  05
Got strange string:  a
Got strange string:  a
Got strange string:  a
Got strange string:  a
Got strange string:  a
```

<br>


`b`的值为什么会存在 "05"或者"a"的情况 ??

<br>

这是因为,`string`类型并不是并发安全的. 


<br>

**string的内部结构：**

```go
struct {
		str uintptr
		len int
	}
```


对 *string* 赋值,并不是原子操作,而是会分为两步,(其实是对上面这个`struct` ，Go 无法保证原子性地完成赋值)

因此可能会出现 *goroutine 1* 刚修改完指针（str）、还没来得及修改长度（len），*goroutine 2* 就读取了这个string 的情况。

str 改了、len 还没来得及改,即 `str="aa",len=1`, 和`str="0",len=2`, 对应"莫名其妙"出现的 "a"和"05"


<br>


```go
func main() {

	var str string = "0"
	p := (*struct {
		str uintptr
		len int
	})(unsafe.Pointer(&str))

	spew.Dump(p)

}
```

<br>

输出:

```rs
(*struct { str uintptr; len int })(0xc00008e2b0)({
 str: (uintptr) 0x1101b01,
 len: (int) 1
})
```


<br>



思考:


*`str="aa",len=1`,变为"a"容易理解,但`str="0",len=2`为什么会是"05"呢?*





<br>



---


<br>


参考:

[Golang string变量操作的原子性](https://www.jianshu.com/p/6dba7d3ecaf1)

[go 字符串string的并发读写的一个坑](https://blog.csdn.net/DisMisPres/article/details/109810231)

[golang: 常用数据类型底层结构分析](https://blog.csdn.net/sryan/article/details/46514883)

[你不知道的 Go 之 string](https://juejin.cn/post/6963639167284690958)

更多相关:

[golang 中 int加一是原子性吗](https://www.baidu.com/s?tn=50000022_hao_pg&ie=utf-8&sc=UWd1pgw-pA7EnHc1FMfqnHRvn1fknWcLPWckPiuW5y99U1Dznzu9m1YknWcvn1RzP103&ssl_sample=normal&srcqid=3302310173687490068&f=3&rsp=6&H123Tmp=nunew7&word=golang+%E4%B8%AD+int%E5%8A%A0%E4%B8%80%E6%98%AF%E5%8E%9F%E5%AD%90%E6%80%A7%E5%90%97)

[golang int并发安全 atom](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&srcqid=3302310173687490068&tn=50000021_hao_pg&wd=golang%20int%E5%B9%B6%E5%8F%91%E5%AE%89%E5%85%A8%20atom&oq=golang%2520int%25E5%25B9%25B6%25E5%258F%2591%25E5%25AE%2589%25E5%2585%25A8&rsv_pq=ef744679000122ed&rsv_t=fd334V%2FJ6LAm1iPQdsxP1LHk9s0qt5Bxtdnuufzhmj5S6m%2FdvyyL%2FLRJrrnyQV81XbDEB82m&rqlang=cn&rsv_enter=1&rsv_sug3=6&rsv_sug1=5&rsv_sug7=100&rsv_sug2=0&rsv_dl=tb&inputT=1157&rsv_sug4=1355
)



姊妹篇:


[golang之map并发访问](https://dashen.tech/2019/01/18/golang%E4%B9%8Bmap%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE/)


[golang之slice并发访问](https://dashen.tech/2019/01/17/golang%E4%B9%8Bslice%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE/)