---
title: 'Golang中select的四大用法 '
date: 2020-03-15 18:14:27
tags: [Go,Pearl]
---

<br>

姊妹篇:

[golang之channel入门](http://www.dashen.tech/2017/12/04/golang%E4%B9%8Bchannel%E5%85%A5%E9%97%A8/)

[golang之channel并发访问](http://www.dashen.tech/2019/01/19/golang%E4%B9%8Bchannel%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE/)

---

### Select vs Switch

<br>

二者有个共同特性就是都通过`case`的方式来处理, 但除此之外几乎完全不同;


`switch..case` 可以处理各种类型，常用来做 接口 interface{} 的判断 (通过variable.(type)). 重点是会依照 case 的顺序依序执行。



```go
package main

import "fmt"

func convert(val interface{}) {

	switch t := val.(type) {
	case int:
		fmt.Println("val为int类型", t)
	case string:
		fmt.Println("val为string类型", t)
	case float64:
		fmt.Println("val为float64类型", t)
	case float32:
		fmt.Println("val为float32类型", t)
	case []string:
		fmt.Println("val为字符串类型的切片", t)
	default:
		fmt.Println("val不是上列类型之一")

	}

}

func main() {

	var i interface{}

	i = float32(3.1415926)
	convert(i)

	i = "dashen"
	convert(i)

	i = 100
	convert(i)

	i = []string{"欧拉", "高斯"}

	convert(i)

}
```


输出为:

```go
val为float32类型 3.1415925
val为string类型 dashen
val为int类型 100
val为字符串类型的切片 [欧拉 高斯]
```

<br>

---


 而`select..case`则只能处理 channel类型

 <font color="blue"><b>即每个 case 必须是一个通信操作, 要么是发送要么是接收</b></font>

 <br>

 select 将随机执行一个可运行的 case。如果没有 case 可运行，它将阻塞，直到有 case 可运行。


- 每个 case 都必须是一个通信
- 所有 channel 表达式都会被求值

- 如果有多个 case 都可以执行，Select 会随机公平地选出一个执行。其他不会执行。<br>
否则：<br>
如果有 default 子句，则执行该语句。<br>
如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。

 ---


<br>


### Select的四大基本用法

<br>

#### <font color="purple">随机选择</font>

<br>

`Random Select `


```go
package main

import "fmt"

func main() {
	var ch1, ch2, ch3 chan int
	var i1, i2 int

	select {
	case i1 = <-ch1:
		fmt.Println("接收到了管道1的一条数据:", i1)
	case ch2 <- i2:
		fmt.Println("向管道2发送了一条数据:", i2)

	case i3, ok := <-ch3:
		if ok {
			fmt.Println("收到了管道3的数据:", i3)
		} else {
			fmt.Println("管道3已被关闭")
		}

	default:
		fmt.Println("以上case皆不可运行,即没有进行通信")
		
	}

}
```

输出为:
```go
以上case皆不可运行,即没有进行通信
```

如果把上述代码中的default语句块去掉,则会报
```go
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [select]:
main.main()
	/Users/shuangcui/go/note/select/2.go:9 +0x108
exit status 2

```


而如果将如上代码中的channel改成有缓存,如:

```go
package main

import "fmt"

func main() {

	ch1 := make(chan int,1)
	ch2 := make(chan int,1)
	ch3 := make(chan int,1)

	ch6 := make(chan int,1)


	var i1, i2 int

	select {
	case i1 = <-ch1:
		fmt.Println("接收到了管道1的一条数据:", i1)
	case ch2 <- i2:
		fmt.Println("向管道2发送了一条数据:", i2)

	case i3, ok := <-ch3:
		if ok {
			fmt.Println("收到了管道3的数据:", i3)
		} else {
			fmt.Println("管道3已被关闭")
		}

	case ch6 <- 271828:
		fmt.Println("向管道6发送了一条数据:", 271828)

	default:
		fmt.Println("以上case皆不可运行,即没有进行通信")

	}

}

```

第二个case和第四个case是可以执行的,所以不会走default语句.输出为:

```go
向管道2发送了一条数据: 0
```

或

```go
向管道6发送了一条数据: 271828
```

二者都满足条件,被select选中执行的概率完全一样

---

<br>

#### <font color="purple">超时控制</b>

<br>

> 假设业务中需调用某接口A，要求超时时间为x秒，那么如何优雅、简洁的实现呢？


利用 `select+time.After`

[参考](https://www.google.com/search?newwindow=1&sxsrf=ALeKk01Lj7LdgheQW4w6RRPsH41cR5KZ8Q%3A1584272554751&ei=qhRuXq2tLfuJk74P5dm_-A4&q=golang+select+%E8%B6%85%E6%97%B6%E6%8E%A7%E5%88%B6&oq=golang+select+%E8%B6%85%E6%97%B6%E6%8E%A7%E5%88%B6&gs_l=psy-ab.3..35i39j0i203l8j0.1558.3683..4082...0.0..0.1161.3823.3-3j6-2j1......0....1..gws-wiz.GO3rQiTTH5w&ved=0ahUKEwitqumWs5zoAhX7xMQBHeXsD-8Q4dUDCAs&uact=5)


```go
package main

import (
	"fmt"
	"time"
)

func main() {

	ch := make(chan string)

	go func() {
		time.Sleep(time.Second * 2)
		ch <- "写入某个值"

	}()

	select {
	case rs := <-ch:
		fmt.Println("结果是:", rs)

	case <-time.After(time.Second * 1):
		fmt.Println("超时!")
	}
}
```

输出为:

```go
超时!
```

参考:

[Go 采用 time.After 实现超时控制](https://shockerli.net/post/golang-select-time-implement-timeout/)

[如何用 go 实现超时控制](https://ictar.xyz/2018/03/20/%E5%A6%82%E4%BD%95%E7%94%A8go%E5%AE%9E%E7%8E%B0%E8%B6%85%E6%97%B6%E6%8E%A7%E5%88%B6/)

[Go语言并发模型：使用 select](https://segmentfault.com/a/1190000006815341)

<br>

关于`time.After`:

```go
// After waits for the duration to elapse and then sends the current time
// on the returned channel.
// It is equivalent to NewTimer(d).C.
// The underlying Timer is not recovered by the garbage collector
// until the timer fires. If efficiency is a concern, use NewTimer
// instead and call Timer.Stop if the timer is no longer needed.
func After(d Duration) <-chan Time {
	return NewTimer(d).C
}
```


接收一个int64类型的值,返回一个Time类型的单向channel


---

<br>


#### <font color="purple">检查channel是否已满</b>

<br>


```go
package main

import "fmt"

func main() {

	ch := make(chan int, 1)

	ch <- 271828

	select {
	case ch <- 31415926:
		fmt.Println("通道的值为:", <-ch)
		fmt.Println("channel vaule is:",<-ch)

	default:
		fmt.Println("通道已经被阻塞,即已经满了")
	}

}
```

输出为:

```go
通道已经被阻塞,即已经满了
```

将如上ch这个int类型通道的缓存值从1改称10,则

```go
ch := make(chan int, 10)
```

则执行结果为:


```go
通道的值为: 271828
channel vaule is: 31415926
```

---

<br>


#### <font color="purple">在循环中使用select</b>

<br>

如果有多个 channel 需要读取, 且读取是不间断的, 就必须使用 `for + select` 机制来实现


```go
package main

import (
	"fmt"
	"time"
)

func main() {

	i := 0

	ch := make(chan string, 0)

	defer func() {
		close(ch)
	}()

	go func() {
	CuiStartLoop: //不加也可以,与后面break后的 CuiStartLoop相呼应,作为循环体的标识
		for {
			time.Sleep(time.Second * 1)
			fmt.Println(time.Now().Unix())
			fmt.Println("当前i的值为:",i)
			i++

			select {

			case m := <-ch:
				fmt.Println("输出为:",m)
				break CuiStartLoop
			default:
				fmt.Println("执行了default代码块")
			}
		}
	}()

	time.Sleep(time.Second * 4)
	ch <- "stop"

}
```



输出:

```go
1584289065
当前i的值为: 0
执行了default代码块
1584289066
当前i的值为: 1
执行了default代码块
1584289067
当前i的值为: 2
执行了default代码块
1584289068
当前i的值为: 3
输出为: stop
```

<br>

当没有值传送进来时, 就会一直停在 select 区段, 所以即便没有 default代码块 也是可以正常运作的.  

而要结束 for 或 select 都需要通过 break 来结束, 但是要在 select 区间直接结束掉 for 循环, 只能使用 break.



---


参考:

[一文掌握 Go 语言 Select 的四大用法](https://mp.weixin.qq.com/s/-i-PoCTPuhRpd4cAKpNwuw)

<br>

更底层(操作系统层面)地进行理解:

[select实现原理](https://draveness.me/golang/docs/part2-foundation/ch05-keyword/golang-select/)


[Go netpoll I/O 多路复用构建原生网络模型之源码深度解析](https://taohuawu.club/go-netpoll-io-multiplexing-reactor?from=timeline)


[一个EOF引发的探索之路之四（理解golang的NetFD之I/O多路复用篇）](https://zhuanlan.zhihu.com/p/53687302?from=timeline&utm_source=wechat_session&utm_medium=social&s_s_i=Jqw9QIb15RPbv1Lyfy3QmB2nKpgFhprNdlq90PvafSQ%3D&s_r=1#showWechatShareTip)



