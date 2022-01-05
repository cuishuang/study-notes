---
title: golang之channel入门
date: 2017-12-04 21:38:38
tags: [Go,Interview]
---

<br>


姊妹篇:

[Golang中select的四大用法](http://www.dashen.tech/2020/03/15/Golang%E4%B8%ADselect%E7%9A%84%E5%9B%9B%E5%A4%A7%E7%94%A8%E6%B3%95/)

[golang之channel并发访问](http://www.dashen.tech/2019/01/19/golang%E4%B9%8Bchannel%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE/)

---


channel是goroutine之间进行通信的桥梁;<br>

借助于 channel, 不同的 goroutine 之间可以相互通信。<br>

channel 是有类型的，而且有方向，可以把 channel 类比成 unix 中的 pipe。<br>

Go 通过 `<-` 操作符来实现 channel 的写和读，向管道 中send value时 `<-` 在 channel 右侧，管道receive value时 `<-` 在左侧，receive value 不赋值给任何变量是合法的。


<b>Channel 最重要的作用就是传递消息。</b>

[理解 Go channel](https://sanyuesha.com/2017/08/03/go-channel/)

---

#### <font color="blue">关闭一个为nil的channel</font>

<br>

<font color="purple">关闭一个nil channel 会导致程序panic</font>

所谓nil channel,即如`var c chan int`这样声明的channel

(与	`c := make(chan int, 0)`相对应)

```go

package main

import "fmt"

func main(){
	var c chan int
	fmt.Println(c)
	close(c)
}
```

执行结果:
```
<nil>

panic: close of nil channel

goroutine 1 [running]:
main.main()
        /Users/shuangcui/xxxxx/channel.go:25 +0x80
exit status 2

```

而<br>

```go
package main

import "fmt"

func main(){
	c := make(chan int, 0)
	fmt.Println(c)
	close(c)
}

```


执行结果为:

```
0xc000064060
```

不会报错


<br>


---

[go 关闭channel分析](https://studygolang.com/articles/9516)

而对于` ch := make(chan int, 0)`这种方式创建的channel,在close时不会panic;  但如果定义后便读取而无写入,则会死锁`fatal error: all goroutines are asleep - deadlock!`;  对于无缓存的channel,只有写入操作也会发生死锁;  对于有缓存的channel,在写入元素数量未达到缓存数时,不会发生报错,代码如下:

```go
package main

import (
	"fmt"
)

func main(){
	ch := make(chan int, 3)
	fmt.Println(ch)
	ch <- 678
	close(ch)
}
```

输出:

```
0xc000096000
```

<br>


#### <font color="blue">读/写一个已经关闭的channel</font>



#### <font color="blue">有两种读取channel的方式,`range`和`<-ch` </font>

<br>

channel关闭之后，**仍然可以从channel中读取剩余的数据，直到数据全部读取完成。**<br>读取完后继续读,得到的将是对应类型的零值.


而 **如果继续向已关闭的channel发送数据，会引起panic**



---
读写nil channel 会永久阻塞从而报fatal error;  

```go

package main

import "fmt"

func main(){
	var c chan int
	fmt.Println(c)
	<- c
}
```

执行结果:
```
<nil>


fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive (nil chan)]:
main.main()
        /Users/shuangcui/xxxxx/channel.go:21 +0x7f
exit status 2


```

---

```go

package main

import "fmt"

func main(){
	var c chan int
	fmt.Println(c)
	c <- 678
}
```

执行结果:
```
<nil>

fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send (nil chan)]:
main.main()
        /Users/shuangcui/xxxxxx/channel.go:23 +0x90
exit status 2

```

---


---

```go
package main

import (
        "fmt"
)


func main(){

        ch := make(chan int, 3)

        ch <-1
        ch <-2
        ch <-3

        close(ch)
        for value := range ch {
                fmt.Println("value:", value)
        }

}
```

输出为:

```
value: 1
value: 2
value: 3
```

---

```go
package main

import (
	"fmt"
)

func main() {

	ch := make(chan int, 3)

	ch <- 1
	ch <- 2
	ch <- 3

	close(ch)

	rs := <-ch

	fmt.Println(rs)
	for value := range ch {
		fmt.Println("value:", value)
	}

	rs1 := <-ch
	fmt.Println(rs1)

}

```

输出为:
```
1
value: 2
value: 3
0
```

---

#### <font color="blue">判断channel是否关闭的方式有两种:</font>

<br>

1. `value, ok := <- ch`,如果ok为false,即说明此ch已关闭

2. for value := range ch {},如果channel被外部关闭,for循环会退出,即range可以识别出channel被关闭


但对一个已关闭的channel，如果继续向channel发送数据，会引起panic。

---

<b>channel不能关闭两次</b>

```go
package main

func main() {
	ch := make(chan bool)
	close(ch)
	close(ch)  // 会panic的，channel不能close两次
}
```

结果:

```
panic: close of closed channel
```


---

#### <font color="blue">使用</font>

<br>


```go
package main
import (
	"fmt"
)

func main() {
	c := make(chan int)
	go func() {
		time.Sleep(10e9)
		fmt.Println("goroutine message")
		c <- 666 //1
	}()
	<-c //2
	fmt.Println("main function message")
}
```

在 goroutine 中, 在代码 #1 处向 channel 发送了数据 666 ，在 main 中 #2 处等待数据的接收，如果 c 中没有数据，代码的执行将发生阻塞，直到有 goroutine 开始往 c 中 send value。

因为有<-c存在,故而在向c写入数据前,程序不会退出;用sync包的WaitGroup可以起到同样的`阻塞`效果;

`这是 channel 最简单的用法之一：同步，这种类型的 channel 容量是 0，称之为 unbuffered channel。`

"对 unbuffered channel 执行 **读 操作** `value := <-ch`, 会一直阻塞直到有数据可接收，执行 **写 操作** `ch <- value`, 也会一直阻塞 直到有 goroutine 对 channel 开始执行接收，正因为如此在同一个 goroutine 中使用 unbuffered channel 会造成 deadlock。"

```go
package main
import (
	"fmt"
)

func main() {
	c := make(chan int)
	c <- 1
	<-c
	fmt.Println("main function message")
}
```

执行报 fatal error: all goroutines are asleep - deadlock! ，读和写相互等待对方从而导致死锁发生。

---


关于带缓存的通道,IO阻塞问题,参见[此文](https://sanyuesha.com/2017/08/03/go-channel/)后半部分

```go
package main
import (
	"fmt"
)

func main() {
	c := make(chan int, 3)
	go func() {
		for i := 0; i < 4; i++ {
			c <- i
			fmt.Println("write to c ", i)
		}
	}()

	for i := 0; i < 4; i++ {
		fmt.Println("reading", <-c)
	}
}
```

"问题在哪里？问题其实是在 fmt.Println ，一次输出就导致 goroutine 的执行发生了切换(相当于发生了 IO 阻塞)，因而即使 c 没有发生阻塞 goroutine 也会让出执行"

即在协程里c<-i的下一行,这个fmt.Println()会发生IO阻塞,当第一次循环,即i=0时,将0写入了c,此时虽然因为未达到缓存的容量3,不会发生阻塞,但fmt.Println()会发生IO阻塞,会向下继续执行

---

"Channel 可以被close关闭  ，channel 关闭之后仍然可以读取，但是向被关闭的 channel send 会 panic。如果 channel 关闭之前有值写入，关闭之后将依次读取 channel 中的消息，读完完毕之后再次读取将会返回 channel 的类型的 zero value"

---


无缓存的channel只有在receiver准备好后send才被执行。如果有缓存，并且缓存未满，则send会被执行。

[Go Channel 详解](https://colobu.com/2016/04/14/Golang-Channels/)
