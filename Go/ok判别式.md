---
title: ok判别式
date: 2017-07-31 22:47:52
tags: Go
---

<br>
类似

```go
if _, ok := map[key]; ok {
//存在
}
```

这样的编译器简化后的判断代码(实际应该是一种**语法糖**),在Go中非常常用. 一般称为称 **"ok判别式"**,但其实这个变量的名称可以随意取,不一定是"ok"


<br>

### 检测map中是否存在某个key

<br>


```go
package main

import "fmt"

func main() {

	isVip := make(map[string]bool)

	isVip["张三"] = true
	isVip["李四"] = false

	if val, ok := isVip["王五"]; ok {
		fmt.Printf("王五是否为vip:%t\n", val)
	} else {
		fmt.Println("王五不是注册用户")
	}

	if val, ok := isVip["李四"]; ok {
		fmt.Printf("李四是否为vip:%t\n", val)
	} else {
		fmt.Println("李四不是注册用户")
	}

}
```

输出为:

```go
王五不是注册用户
李四是否为vip:false
```


<br>

---


<br>



### 类型断言:检测一个接口类型的变量i,是否包含了类型T

<br>

```go
if value, ok := i.(T); ok {
    fmt.Println(value)
} else{
 // 接口类型i没有包含类型T
}

```

更多可参见:

[Golang类型断言]()


<br>

---

<br>

### ~~检测一个通道是否关闭~~

<br>

更确切说,实际是检测一个通道还能否继续读取.

即对于有缓存的通道ch,在close(ch)之后,如果还有未读取出的变量,ok判别式的值为`true`.

即改为: **检测一个通道能否继续读取**更为合适


---

<br>

### 检测是否还能从通道中读取出传递的值


<br>


可参考:

[golang之channel入门](http://www.dashen.tech/2017/12/04/golang%E4%B9%8Bchannel%E5%85%A5%E9%97%A8/)


<br>

```go
package main

import "fmt"

func main() {

	ch := make(chan int, 2)

	fmt.Println(ch)

	ch <- 271828

	fmt.Println(<-ch) //这样也相当于进行了读取,channel里的缓存空间又会增加1个单位

	ch <- 271828  //#1
	ch <- 271828  //#2

	//channel<-271828 //如果继续写入,超过缓存值2,则会报fatal error: all goroutines are asleep - deadlock!

	close(ch)

	//关闭之后尝试进行读取
	//channel关闭之后，仍然可以从channel中读取剩余的数据，直到数据全部读取完成。
	//读取完后继续读,得到的将是对应类型的零值.

	if val, ok := <-ch; ok {
		fmt.Println("值为:", val)
	} else {
		fmt.Println("通道已被关闭,无法再进行读取")
	}

}
```

输出为i:
```go
0xc00009a000
271828
值为: 271828
```

<br>

而如果将两行写缓存的代码注释掉,即注释掉#1和#2,则输出结果为:


```go
0xc000090000
271828
通道已被关闭,无法再进行读取
```


<br>

即如果还能读出缓存的值,即便通道已经被关闭,ok的值依然会是`true`


---

<br>



参考:




[golang中逗号ok模式_转](https://www.cnblogs.com/embedded-linux/p/11129103.html)



[Go语言那些返回值数量变化的语句？](https://www.jianshu.com/p/9a98b0f5ffdb)
