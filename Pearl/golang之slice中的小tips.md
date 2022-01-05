---
title: golang之slice中的小tips
date: 2010-03-02 23:44:28
tags: [Go,Pearl]
---

<br>

参考:

[Golang 之 切片](https://blog.csdn.net/icebergliu1234/article/details/105103380)

<br>



### 使用make()构造切片

<br>

语法为:

> make ([]T, size, cap)<br>
T:切片的元素类型<br>
size:切片中元素的数量<br>
cap:切片的容量


```go
package main

import "fmt"

func main() {
	a := make([]int, 2, 10)
	fmt.Printf("len(a):%d,cap(a):%d\n", len(a), cap(a))
}

----------
len(a):2,cap(a):10
```


<br>

**如果不写容量，则默认长度就是容量。**
```go
package main
 
import "fmt"
 
func main() {
    a := make([]int, 2)
    fmt.Printf("len(a):%d,cap(a):%d\n", len(a), cap(a))
}
----------
len(a):2,cap(a):2
```

<br>

---

<br>

### 切片的比较

<br>

切片之间不能比较, 不能使用`==`操作符来判断两个切片是否含有全部相等元素.

切片唯一合法的比较操作是和nil比较.一个nil值的切片并没有底层数组,一个nil值的切片的长度和容量都是0.

<br>

---

<br>

### 基于数组定义切片 & 基于切片再切片 情况下的各种陷阱

<br>

```go
package main

import "fmt"

func main() {

	//定义一个包含10个int型元素的数组
	a := [10]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	// 或定义为
	//a := [...]int{0,1,2,3,4,5,6,7,8,9}

	//得到[0 1 2 3 4 5 6 7 8 9]
	fmt.Println(a)

	// 左闭右开,
	sli1 := a[3:6]

	// sli为[3,4,5]
	fmt.Println(sli1)

	//切片sli1的长度为3,容量为7---这是因为 容量是从数组中切片的首元素下标开始数, (默认)数到数组的尾下标
	//sli为[3,4,5],其底层数组为[3 4 5 6 7 8 9]
	fmt.Println(len(sli1), cap(sli1))

	//可以通过第三个参数 来控制切片的容量
	//结果为3,4
	fmt.Println(len(a[3:6:7]), cap(a[3:6:7]))

	//基于切片再切片, 并不是在原来的切片上面切片, 因为切片是引用类型, 所以再切片也是在底层数组上进行切分的.
	//再切片不能超过数组的长度,否则会panic:越界
	//同时如果切片限制了容量, 那么再切片自然也不能超过这个容量,否则同样会报panic:越界..

	//此时sli1的底层数组为[...]int{3,4,5,6,7,8,9}
	sli2 := sli1[4:7]
	//输出为[7,8,9]
	fmt.Println(sli2)

	// panic: runtime error: slice bounds out of range [:8] with capacity 7
	//即容量为7,却切到了第8个元素,发生了越界
	// fmt.Println(sli1[4:8])

	// 修改切片中元素的值,会不会影响底层数组?及由该底层数组切分出的其他切片?
	// 切片是引用类型,所以是会影响的

	sli3 := sli1[1:3]
	fmt.Println("sli3的值为:", sli3) // [4 5]

	//sli为[3,4,5],其底层数组为[3 4 5 6 7 8 9]
	fmt.Println(sli1[2]) //输出为5
	sli1[2] = 10086

	fmt.Println(a)    //为 [0 1 2 3 4 10086 6 7 8 9]
	fmt.Println(sli3) //为 [4 10086]

}
```

<font color="orange">切片是引用类型，只要它们是从同一个底层"链接"出去的，那其修改操作就会影响底层。</font>


再如:

```go
package main

import "fmt"

var s []int

func main() {

	s = append(s, 1)
	s = append(s, 2)
	s = append(s, 3)
	s = append(s, 4)
	s = append(s, 5)
	s = append(s, 6)
	s = append(s, 7)
	s = append(s, 8)
	s = append(s, 9)

	//此时s为[1,2,3,4,5,6,7,8,9]

	x := getBuff() //x的值为[2],容量为8,底层数组为[2,3,4,5,6,7,8,9]

	fmt.Println(x) 

	x[0] = 666

	fmt.Println(x) //x为666

	fmt.Println(s) //[1,666,3,4,5,6,7,8,9]

}

func getBuff() []int {
	b := s[1:2]
	fmt.Println("s的值为:", s)

	return b

}
```

输出为:

```go
s的值为: [1 2 3 4 5 6 7 8 9]
[2]
[666]
[1 666 3 4 5 6 7 8 9]
```


<br>


---

<br>

### 使用copy(深度)拷贝切片

<br>

```go
package main

import "fmt"

func main() {
	a := []int{1, 2, 3, 4, 5}
	b := a
	c := make([]int, 5, 5) //新(独立)切片的长度和容量,都不能小于要copy的切片,否则会报 index out of range
	copy(c, a)             //使用copy()函数将切片a中的元素复制到切片c
	fmt.Printf("a:%v,len(a):%d,cap(a):%d\n", a, len(a), cap(a))
	fmt.Printf("c:%v,len(c):%d,cap(c):%d\n", c, len(c), cap(c))
	c[0] = 1000 // copy操作之后的切片c和切片a之间没有任何关系 是两个独立的切片
	fmt.Printf("a:%v,len(a):%d,cap(a):%d\n", a, len(a), cap(a))
	fmt.Printf("c:%v,len(c):%d,cap(c):%d\n", c, len(c), cap(c))

	a[0] = 666

	fmt.Println("b的值为:", b)

}

//--------
//a:[1 2 3 4 5],len(a):5,cap(a):5
//c:[1 2 3 4 5],len(c):5,cap(c):5
//a:[1 2 3 4 5],len(a):5,cap(a):5
//c:[1000 2 3 4 5],len(c):5,cap(c):5
//b的值为: [666 2 3 4 5]

```

<font color="orange">**copy操作之后的切片c和切片a之间没有任何关系 是两个独立的切片**</font>


<br>


---


<br>

### !!!append及切片的扩容策略

<br>


- 1.如果要的容量没有原来容量两倍大, 那就扩充到原来容量的两倍. 

- 2.如果要的容量是原来容量的两倍还要多, 那新的容量就是所要求的容量大小.

- 3.如果原来的容量大于1024,那么每次提升25%,不再是两倍扩容.

- 4.还有更复杂的内存对齐问题,可参考 [golang中slice扩容一定是double或1.25倍吗](http://www.dashen.tech/2019/06/28/golang%E4%B8%ADslice%E6%89%A9%E5%AE%B9%E4%B8%80%E5%AE%9A%E6%98%AFdouble%E6%88%961-25%E5%80%8D%E5%90%97/)

<br>

---

1. 双倍

```go
package main

import "fmt"

func main() {

	var s []int

	s = append(s, 1)
	fmt.Println("此时的长度和容量为:111", len(s), cap(s))
	s = append(s, 2)
	fmt.Println("此时的长度和容量为:222", len(s), cap(s))
	s = append(s, 3)
	fmt.Println("此时的长度和容量为:333", len(s), cap(s))
	s = append(s, 4)
	fmt.Println("此时的长度和容量为:444", len(s), cap(s))
	s = append(s, 5)
	fmt.Println("此时的长度和容量为:555", len(s), cap(s))
	s = append(s, 6)
	fmt.Println("此时的长度和容量为:666", len(s), cap(s))
	s = append(s, 7)
	fmt.Println("此时的长度和容量为:777", len(s), cap(s))
	s = append(s, 8)
	fmt.Println("此时的长度和容量为:888", len(s), cap(s))
	s = append(s, 9)
	fmt.Println("此时的长度和容量为:999", len(s), cap(s))

	fmt.Println("此时s的值为:", s) //此时s的值为: [1 2 3 4 5 6 7 8 9],长度为9,容量为16; 小于1024个元素时两倍扩容

	x := getBuff1(s)

	fmt.Println("x的值为:", x) //[2]

	x[0] = 666

	fmt.Println("此时x的值为:", x) //[666]

	fmt.Println("此时s的值为:", s) //[1 666 3 4 5 6 7 8 9]
	fmt.Println("此时s的长度和容量为:", len(s), cap(s))

}

func getBuff1(s []int) []int {

	fmt.Println("s的容量为:", cap(s))
	b := s[1:2] //b的值为2,底层数组为[2,3,4,5,6,7,8,9, 0,0,0,0,0,0,0]

	fmt.Println("s[:16]的值为:", s[:16]) //s[:16]的值为: [1 2 3 4 5 6 7 8 9 0 0 0 0 0 0 0]

	fmt.Println("b的容量为:", cap(b)) //15

	//可用第三个参数指定容量,即如c:=s[1:2:4]
	c := s[1:2:4]
	fmt.Println("c的长度和容量为:", len(c), cap(c)) //c的长度和容量为: 1 3

	return b

}
```

输出为:

```go
此时的长度和容量为:111 1 1
此时的长度和容量为:222 2 2
此时的长度和容量为:333 3 4
此时的长度和容量为:444 4 4
此时的长度和容量为:555 5 8
此时的长度和容量为:666 6 8
此时的长度和容量为:777 7 8
此时的长度和容量为:888 8 8
此时的长度和容量为:999 9 16
此时s的值为: [1 2 3 4 5 6 7 8 9]
s的容量为: 16
s[:16]的值为: [1 2 3 4 5 6 7 8 9 0 0 0 0 0 0 0]
b的容量为: 15
c的长度和容量为: 1 3
x的值为: [2]
此时x的值为: [666]
此时s的值为: [1 666 3 4 5 6 7 8 9]
此时s的长度和容量为: 9 16
```


<br>


再如:

```go
package main

import "fmt"

func main(){
	s := []int{1,2,3,4,5,6}

	fmt.Println(len(s))//6
	fmt.Println(cap(s))//6

	s = append(s,100)
	fmt.Println(len(s))//7
	fmt.Println(cap(s))//12

	s6 := s[2:5]  //[3,4,5]

	fmt.Println(s6)//因为没有置顶第三个参数,所以切到底, 其底层数组为[3 4 5 6 100 0 0 0 0 0]

	fmt.Println(s6[:10]) //[3 4 5 6 100 0 0 0 0 0]

	fmt.Println(cap(s6))//10
}
```
输出为:
```go
6
6
7
12
[3 4 5]
[3 4 5 6 100 0 0 0 0 0]
10
```

<br>


---

2. 一次压入多个元素, 要的容量大于原容量的两倍,则新容量就是其需要的容量


<br>

```go
package main

import "fmt"

func main() {
	s1 := []string{"北京", "上海", "深圳"}
	fmt.Printf("len(s1):%d,cap(s1):%d\n", len(s1), cap(s1))
	s1 = append(s1, "广州", "成都", "重庆", "石家庄", "保定", "邢台", "张家口")
	fmt.Printf("len(s1):%d,cap(s1):%d\n", len(s1), cap(s1))
}

----------
len(s1):3,cap(s1):3
len(s1):10,cap(s1):10
```

<br>

---


3. 元素个数大于1024时,变为1.25倍扩容

<br>


```go
package main

import "fmt"

func main() {

	sli := make([]int, 0)
	fmt.Println("最开始sli长度和容量为:", len(sli), cap(sli)) //0,0

	for i := 0; i < 1024; i++ {
		sli = append(sli, i)
	}

	fmt.Println("循环之后sli长度和容量为:", len(sli), cap(sli)) //1024,1024

	sli = append(sli, 10086)

	fmt.Println("最终sli长度和容量为:", len(sli), cap(sli)) //1025,1280

}
```


---


<br>

### !!!扩容时底层数组的变化

<br>

```go
package main

import "fmt"

func main() {
	var numSlice []int
	for i := 0; i < 10; i++ {
		numSlice = append(numSlice, i)
		fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", numSlice, len(numSlice), cap(numSlice), numSlice)
	}
}
```

输出为:

```go
[0]  len:1  cap:1  ptr:0xc000016068

[0 1]  len:2  cap:2  ptr:0xc0000160a0

[0 1 2]  len:3  cap:4  ptr:0xc000014160
[0 1 2 3]  len:4  cap:4  ptr:0xc000014160

[0 1 2 3 4]  len:5  cap:8  ptr:0xc0000181c0
[0 1 2 3 4 5]  len:6  cap:8  ptr:0xc0000181c0
[0 1 2 3 4 5 6]  len:7  cap:8  ptr:0xc0000181c0
[0 1 2 3 4 5 6 7]  len:8  cap:8  ptr:0xc0000181c0

[0 1 2 3 4 5 6 7 8]  len:9  cap:16  ptr:0xc000080000
[0 1 2 3 4 5 6 7 8 9]  len:10  cap:16  ptr:0xc000080000
```


<br>

<font color="orange">共享同一个底层数组时,切片2修改值,会影响切片1的值; 当两个切片不再共享一个底层数组时,修改就互不影响.</font>

如下:

```go
package main

import "fmt"

func main() {

	var sli []int

	for i := 0; i < 4; i++ {
		sli = append(sli, i)
		fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli, len(sli), cap(sli), sli)
	}

	fmt.Println("---------")

	sli2 := sli[:]

	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli2, len(sli2), cap(sli2), sli2)
	sli2[0] = 10086
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli2, len(sli2), cap(sli2), sli2)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli, len(sli), cap(sli), sli)

	sli = append(sli, 666)

	fmt.Println("==========")
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli, len(sli), cap(sli), sli)
	sli2[2] = 271828

	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli2, len(sli2), cap(sli2), sli2)

	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli, len(sli), cap(sli), sli)

}

```
输出为:

```go
[0]  len:1  cap:1  ptr:0xc00008c008
[0 1]  len:2  cap:2  ptr:0xc00008c040
[0 1 2]  len:3  cap:4  ptr:0xc000092020
[0 1 2 3]  len:4  cap:4  ptr:0xc000092020
---------
[0 1 2 3]  len:4  cap:4  ptr:0xc000092020
[10086 1 2 3]  len:4  cap:4  ptr:0xc000092020
[10086 1 2 3]  len:4  cap:4  ptr:0xc000092020
==========
[10086 1 2 3 666]  len:5  cap:8  ptr:0xc000084080
[10086 1 271828 3]  len:4  cap:4  ptr:0xc000092020
[10086 1 2 3 666]  len:5  cap:8  ptr:0xc000084080
```


<br>



再如:(原因待解)

```go
func main() {

	var sli []int


	sli1 := append(sli, 1)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli1, len(sli1), cap(sli1), sli1)

	sli2 := append(sli, 1, 2)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli2, len(sli2), cap(sli2), sli2)

	sli3 := append(sli,1,2,3)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli3, len(sli3), cap(sli3), sli3)


	sli4 := append(sli,1,2,3,4)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli4, len(sli4), cap(sli4), sli4)


	sli5 := append(sli,1,2,3,4,5)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli5, len(sli5), cap(sli5), sli5)


	sli6 := append(sli,1,2,3,4,5,6)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli6, len(sli6), cap(sli6), sli6)


	sli7 := append(sli,1,2,3,4,5,6,7)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli7, len(sli7), cap(sli7), sli7)


	sli8 := append(sli,1,2,3,4,5,6,7,8)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli8, len(sli8), cap(sli8), sli8)


	sli9 := append(sli,1,2,3,4,5,6,7,8,9)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli9, len(sli9), cap(sli9), sli9)


	sli10 := append(sli,1,2,3,4,5,6,7,8,9,10)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli10, len(sli10), cap(sli10), sli10)



	sli11 := append(sli,1,2,3,4,5,6,7,8,9,10,11)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli11, len(sli11), cap(sli11), sli11)

	sli12 := append(sli,1,2,3,4,5,6,7,8,9,10,11,12)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli12, len(sli12), cap(sli12), sli12)


	sli13 := append(sli,1,2,3,4,5,6,7,8,9,10,11,12,13)
	fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", sli13, len(sli13), cap(sli13), sli13)

}
```

输出为:

```go
[1]  len:1  cap:1  ptr:0xc00008c008
[1 2]  len:2  cap:2  ptr:0xc00008c040
[1 2 3]  len:3  cap:4  ptr:0xc00009a020
[1 2 3 4]  len:4  cap:4  ptr:0xc00009a040
[1 2 3 4 5]  len:5  cap:6  ptr:0xc00008e030
[1 2 3 4 5 6]  len:6  cap:6  ptr:0xc00008e060
[1 2 3 4 5 6 7]  len:7  cap:8  ptr:0xc000084080
[1 2 3 4 5 6 7 8]  len:8  cap:8  ptr:0xc0000840c0
[1 2 3 4 5 6 7 8 9]  len:9  cap:10  ptr:0xc00009c000
[1 2 3 4 5 6 7 8 9 10]  len:10  cap:10  ptr:0xc00009c050
[1 2 3 4 5 6 7 8 9 10 11]  len:11  cap:12  ptr:0xc000064060
[1 2 3 4 5 6 7 8 9 10 11 12]  len:12  cap:12  ptr:0xc0000640c0
[1 2 3 4 5 6 7 8 9 10 11 12 13]  len:13  cap:14  ptr:0xc00009e000
```


个中原因,//TODO


<br>


---

<br>


### 对 数组/切片 进行排序

<br>

可以使用自带的`sort`包进行排序,但入参必须是切片.所以如果是数组a1排序,要传入其对应的切片a1[:]

```go
package main

import (
	"fmt"
	"sort"
)

func main() {

	var a1 = [...]int{3, 7, 8, 9, 1}
	sort.Ints(a1[:])
	fmt.Println(a1)

	a2 := [...]string{"v", "u", "a", "f"}
	sort.Strings(a2[:])
	fmt.Println(a2)
}

//-----
//[1 3 7 8 9]
//[a f u v]
```



<br>

---


更多关于slice的"黑魔法",
<br>

参考:

[[译]Go Slice 秘籍](https://colobu.com/2017/03/22/Slice-Tricks/)