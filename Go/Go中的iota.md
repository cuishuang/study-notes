---
title: Go中的iota
date: 2019-07-14 21:58:53
tags: Go
---


### 基本使用


```go
package main

import "fmt"

const a0 = iota // a0 = 0  // const出现, iota初始化为0

const (
	a1 = iota // a1 = 0   // 又一个const出现, iota初始化为0
	a2 = iota // a2 = 1   // const新增一行, iota 加1
	a3 = 6    // a3 = 6   // 自定义一个常量
	a4        // a4 = 6   // 不赋值就和上一行相同
	a5 = iota // a5 = 4   // const已经新增了4行, 所以这里是4
)

const (
	b = iota // b = 0
	c        // c = 1
	d = 1111
	e
	f = iota
)

const (
	TestMin = -1
	TestA
	TestB = iota
	TestC
)

func main() {

	fmt.Println("a0:", a0)
	fmt.Println("a1:", a1)
	fmt.Println("a2:", a2)
	fmt.Println("a3:", a3)
	fmt.Println("a4:", a4)
	fmt.Println("a5:", a5)

	fmt.Println("---------")

	fmt.Println("b is:", b)
	fmt.Println("c is:", c)
	fmt.Println("d is:", d)
	fmt.Println("e is:", e)
	fmt.Println("f is:", f)

	fmt.Println("---------")

	fmt.Println("TestMin:", TestMin)
	fmt.Println("TestA:", TestA)
	fmt.Println("TestB:", TestB)
	fmt.Println("TestC:", TestC)

}

```

<br>

输出为：

```go
a0: 0
a1: 0
a2: 1
a3: 6
a4: 6
a5: 4
---------
b is: 0
c is: 1
d is: 1111
e is: 1111
f is: 4
---------
TestMin: -1
TestA: -1
TestB: 2
TestC: 3
```

<br>


```go
package main

import "fmt"

const (
	TestMin = -1
	TestA
	_
	TestB = iota
	TestC
)

func main() {
	fmt.Println(TestMin)
	fmt.Println(TestA)
	fmt.Println(TestB)
	fmt.Println(TestC)
}
```

输出为：

```go
-1
-1
3
4
```

<br>

```go
package main

import "fmt"


// 跳值使用iota使用_来达到目的：
const (
	h = iota // 0
	i = iota // 1
	_
	k = iota // 3
)

// 当常量表达式为空时，会自动继承上一个存在非空的表达式。
const (
	x = iota * 2
	y
	z
)

func main() {
	fmt.Println(h)
	fmt.Println(i)
	fmt.Println(k)

	fmt.Println("-------")

	fmt.Println(x)
	fmt.Println(y)
	fmt.Println(z)
}

```

输出为

```go
0
1
3
-------
0
2
4
```


<br>

### 用来定义枚举值

```go
package main

import "fmt"

type OrderState uint8

const (
	// 0-待支付
	WaitPay OrderState = iota
	// 1-支付中
	Paying
	// 2-支付成功
	PaySucc
	// 3-支付失败
	PayFail
)

// 对应于xx表is_show字段
const (
	_       uint8 = iota
	Show          // 1-显示
	NotShow       // 2-不显示
)

func main() {

	fmt.Println(PaySucc)

	fmt.Println(Show)
}

```

输出为

```go
2
1
```

<br>

### 高阶用法


```go
package main

import (
	"fmt"
)

const (
	i = 1 << iota
	j = 3 << iota
	k
	l
)

func main() {
	fmt.Println("i=", i)
	fmt.Println("j=", j)
	fmt.Println("k=", k)
	fmt.Println("l=", l)
}

```

输出为

```go
i= 1
j= 6
k= 12
l= 24
```

iota每出现一次，自动加1；而前面的操作数如果不指定，默认使用上一个的，在这里是3；

即

```go
    i=1<<iota
    j=3<<iota
    k
    l
```

等价于

```go
    i=1<<0
    j=3<<1
    k=3<<2
    l=3<<3
```

<br>


又如

```go
package main

import "fmt"

func main() {
	const (
		IgEggs         = 1 << iota // 1 << 0 which is 00000001
		IgChocolate                // 1 << 1 which is 00000010
		IgNuts                     // 1 << 2 which is 00000100
		IgStrawberries             // 1 << 3 which is 00001000
		IgShellfish                // 1 << 4 which is 00010000
	)
	fmt.Println(IgEggs, IgChocolate, IgNuts, IgStrawberries, IgShellfish)
}
```

输出为

`1 2 4 8 16`

<br>


**每次可以左移一位，因此对于定义数量级大有裨益**


```go
package main

import "fmt"

type ByteSize int64

const (
	_           = iota             // ignore first value by assigning to blank identifier
	KB ByteSize = 1 << (10 * iota) // 1 << (10*1)
	MB                             // 1 << (10*2)
	GB                             // 1 << (10*3)
	TB                             // 1 << (10*4)
	PB                             // 1 << (10*5)
	EB                             // 1 << (10*6)
	//ZB                             // 1 << (10*7)
	//YB                             // 1 << (10*8)
)

func main() {
	fmt.Printf("KB= %d Byte\n", KB)
	fmt.Printf("MB= %d Byte\n", MB)
	fmt.Printf("GB= %d Byte\n", GB)
}

```

输出为

```go
KB= 1024 Byte
MB= 1048576 Byte
GB= 1073741824 Byte
```


