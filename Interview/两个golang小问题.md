---
title: 两个golang小问题
date: 2020-08-05 08:03:36
tags: [Go,Interview]
---



### 切片的长度和容量

```go
package main

import "fmt"


func main(){

	a := []byte("0123456789")

	fmt.Printf("[a_src] len: %v\n", len(a))
	fmt.Printf("[a_src] cap: %v\n", cap(a))

	b := make([]byte, 5)
	copy(b, a[:5])
	fmt.Printf("[b_cpy] len: %v\n", len(b))
	fmt.Printf("[b_cpy] cap: %v\n", cap(b))

	a = a[:5]

	fmt.Printf("[a_dst] len: %v\n", len(a))
	fmt.Printf("[a_dst] cap: %v\n", cap(a))


}

```

<br>

结果为：

```
[a_src] len: 10
[a_src] cap: 10
[b_cpy] len: 5
[b_cpy] cap: 5
[a_dst] len: 5
[a_dst] cap: 10
```


<br>

参考：

[golang中slice扩容一定是double或1.25倍吗](http://www.dashen.tech/2019/06/28/golang%E4%B8%ADslice%E6%89%A9%E5%AE%B9%E4%B8%80%E5%AE%9A%E6%98%AFdouble%E6%88%961-25%E5%80%8D%E5%90%97/)

---


<br>



### defer，以及return不是原子操作的问题


```go
package main

import "fmt"

func Add200(z *int) int {
	fmt.Println("Add200:", *z)
	*z += 200
	return *z
}

func deferRet(x, y int) (z int) {
	defer func() {
		fmt.Println("func:", z)
		z += 100
	}()
	z = x + y
	return Add200(&z)
}

func main() {
	i := deferRet(1, 1)
	fmt.Println("main:", i)
}

```


<br>

结果为：

```
Add200: 2
func: 202
main: 302
```


参考：

[Golang中的defer](http://www.dashen.tech/2019/08/24/Golang%E4%B8%AD%E7%9A%84defer/)

