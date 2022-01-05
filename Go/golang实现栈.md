---
title: golang实现栈
date: 2017-04-12 00:43:08
tags: [数据结构,Go]
---

<br>

参考:

[Golang实现数据结构“栈”的三种实现，性能对比及应用示例](https://hansedong.github.io/2019/04/02/15/)


---

<br>


### <font style="color:orangered;font-weight:bold;">方法1: 利用Golang的slice(相当于利用数组)</font>

<br>


**1.go**

```go
package main

import (
	"fmt"
	"sync"
)

// Item the type of the stack
type Item interface{}

// ItemStack the stack of Items
type ItemStack struct {
	items []Item
	lock  sync.RWMutex
}

// New creates a new ItemStack
func NewStack() *ItemStack {
	s := &ItemStack{}
	s.items = []Item{}
	return s
}

// Pirnt prints all the elements
func (s *ItemStack) Print() {
	fmt.Println(s.items)
}

// Push adds an Item to the top of the stack
func (s *ItemStack) Push(t Item) {
	s.lock.Lock()
	defer s.lock.Unlock()
	s.items = append(s.items, t)
}

// Pop removes an Item from the top of the stack
func (s *ItemStack) Pop() Item {
	s.lock.Lock()
	defer s.lock.Unlock()
	if len(s.items) == 0 {
		return nil
	}
	item := s.items[len(s.items)-1]
	s.items = s.items[0 : len(s.items)-1]
	return item
}

func main() {

	s := NewStack()

	s.Push([]int{1, 4, 7})
	s.Push([]int{5, 8, 9})
	//fmt.Println(s)
	s.Print()

	s.Pop()
	//fmt.Println(s)
	s.Print()
}
```

<br>


**1_test.go**

```go
package main

import (
	"strconv"
	"testing"
)

var stack *ItemStack

func init() {
	stack = NewStack()
}

func Benchmark_Push(b *testing.B) {
	for i := 0; i < b.N; i++ { //use b.N for looping
		now := strconv.Itoa(i)
		stack.Push("test" + now)
	}

	//fmt.Println(stack)
}

func Benchmark_Pop(b *testing.B) {
	b.StopTimer() //类似时间锁,在StopTimer()和StartTimer()之见的代码,不计入测量
	for i := 0; i < b.N; i++ { //use b.N for looping
		now := strconv.Itoa(i)
		stack.Push("test" + now)
	}
	b.StartTimer()
	for i := 0; i < b.N; i++ { //use b.N for looping
		stack.Pop()
	}
}
```

<br>

关于基准测试,参考:

[Go测试](http://www.dashen.tech/2016/04/12/Go%E6%B5%8B%E8%AF%95/)

<br>


` go test -test.bench=".*" -benchmem -v`

结果如下:

```go
goos: darwin
goarch: amd64
pkg: 上级目录/当前目录
Benchmark_Push-4         3611212               379 ns/op             130 B/op          2 allocs/op
Benchmark_Pop-4         20186299                52.2 ns/op             0 B/op          0 allocs/op
PASS
ok      上级目录/当前目录 12.202s
```

<br>


---


<br>


### <font style="color:orangered;font-weight:bold;">方法2: 利用Golang的container/list内置包(即利用链表)</font>

<br>

关于Go的list,参见[golang中list的源码](http://www.dashen.tech/2012/01/01/golang%E4%B8%ADlist%E7%9A%84%E6%BA%90%E7%A0%81/)

<br>

**2.go**

```go
package main

import (
	"container/list"
	"fmt"
	"sync"
)

type Stack struct {
	list *list.List
	lock *sync.RWMutex
}

func NewStack() *Stack {
	aList := list.New()
	l := &sync.RWMutex{}
	return &Stack{aList, l}
}

func (stack *Stack) Push(value interface{}) {
	stack.lock.Lock()
	defer stack.lock.Unlock()
	stack.list.PushBack(value)
}

func (stack *Stack) Pop() interface{} {
	stack.lock.Lock()
	defer stack.lock.Unlock()
	e := stack.list.Back()
	if e != nil {
		stack.list.Remove(e)
		return e.Value
	}
	return nil
}

func (stack *Stack) Peak() interface{} {
	e := stack.list.Back()
	if e != nil {
		return e.Value
	}

	return nil
}

func (stack *Stack) Len() int {
	return stack.list.Len()
}

func (stack *Stack) Empty() bool {
	return stack.list.Len() == 0
}


func main() {

	s := NewStack()

	s.Push([]int{1, 4, 7})
	s.Push([]int{5, 8})
	s.Push([]string{"a","d","f","g","w"})

	fmt.Println(s)
	fmt.Println(s.Len())

	fmt.Println(s.list.Front().Value)
	fmt.Println(s.list.Back().Value)

	s.list.InsertBefore([]string{"cuishuang, dashen"},s.list.Back())

	fmt.Println(s.Len())

	fmt.Println(s.list.Back().Value)
	fmt.Println(s.list.Back().Prev())
	fmt.Println(s.list.Back().Next())

	fmt.Println(s.Len())
	s.Pop()
	fmt.Println(s.Len())
	fmt.Println(s.list.Front().Value)

}
```

输出为:

```go
&{0xc00006a180 0xc000092000}
3
[1 4 7]
[a d f g w]
4
[a d f g w]
&{0xc00006a210 0xc00006a1e0 0xc00006a180 [cuishuang, dashen]}
<nil>
4
3
[1 4 7]
```

<br>


**2_test.go**

```go
package main

import (
	"strconv"
	"testing"
)

var stack *Stack

func init() {
	stack = NewStack()
}

func Benchmark_Push(b *testing.B) {
	for i := 0; i < b.N; i++ { //use b.N for looping
		now := strconv.Itoa(i)
		stack.Push("test" + now)
	}

	//fmt.Println(stack)
}

func Benchmark_Pop(b *testing.B) {
	b.StopTimer() //类似时间锁,在StopTimer()和StartTimer()之见的代码,不计入测量
	for i := 0; i < b.N; i++ { //use b.N for looping
		now := strconv.Itoa(i)
		stack.Push("test" + now)
	}
	b.StartTimer()
	for i := 0; i < b.N; i++ { //use b.N for looping
		stack.Pop()
	}
}
```

` go test -test.bench=".*" -benchmem -v`

结果如下:

```go
goos: darwin
goarch: amd64
pkg: 上级目录/当前目录
Benchmark_Push-4         3439525               409 ns/op              85 B/op          3 allocs/op
Benchmark_Pop-4         20381214                54.8 ns/op             0 B/op          0 allocs/op
PASS
ok      上级目录/当前目录 14.733s
```

<br>


---


<br>


### <font style="color:orangered;font-weight:bold;">方法3: godoc的实现（自定义数据结构）</font>


<br>

**3.go**

```go
package main

import (
	"fmt"
	"sync"
)

type (
	Stack struct {
		top    *node
		length int
		lock   *sync.RWMutex
	}
	node struct {
		value interface{}
		prev  *node
	}
)

// Create a new stack
func NewStack() *Stack {
	return &Stack{nil, 0, &sync.RWMutex{}}
}

// Return the number of items in the stack
func (this *Stack) Len() int {
	return this.length
}

// View the top item on the stack
func (this *Stack) Peek() interface{} {
	if this.length == 0 {
		return nil
	}
	return this.top.value
}

// Pop the top item of the stack and return it
func (this *Stack) Pop() interface{} {
	this.lock.Lock()
	defer this.lock.Unlock()
	if this.length == 0 {
		return nil
	}
	n := this.top
	this.top = n.prev
	this.length--
	return n.value
}

// Push a value onto the top of the stack
func (this *Stack) Push(value interface{}) {
	this.lock.Lock()
	defer this.lock.Unlock()
	n := &node{value, this.top}
	this.top = n
	this.length++
}

func main() {

	s := NewStack()

	s.Push([]int{1, 4, 7, 0, 11, -3, 9})
	s.Push([]int{5})
	s.Push([]string{"a", "d", "f", "g", "w"})

	fmt.Println(s.Peek())
	fmt.Println(s.Len())

	s.Pop()

	fmt.Println(s.Peek())
	fmt.Println(s.Len())

}

```

输出为:

```go
[a d f g w]
3
[5]
2
```

<br>

**3_test.go** 同 *2_test.go*


` go test -test.bench=".*" -benchmem -v`

结果如下:

```go
goos: darwin
goarch: amd64
pkg: l上级目录/当前目录
Benchmark_Push-4         3468397               361 ns/op              69 B/op          3 allocs/op
Benchmark_Pop-4         24239797                52.3 ns/op             0 B/op          0 allocs/op
PASS
ok      上级目录/当前目录 16.249s
```


<br>


---

<br>

### <font style="color:orangered;font-weight:bold;">性能对比:</font>


<br>


| 实现方式       | push速度 |    pop速度 | push内存分配| pop内存分配
|:-----------|:----:| :-------------:| :--------:| :--------|
| 基于slice  |  379 ns/op  |     52.2 ns/op | 130 B/op | 0 B/op |
| container/list链表 |  409 ns/op  |   54.8 ns/op | 85 B/op | 0 B/op |
| 自定义数据结构 |  361 ns/op  | 52.3 ns/op | 68 B/op | 0 B/op|




---

<br>

更多:

[【golang】用container/list实现栈（Stack）](https://studygolang.com/articles/281)


[Google结果](https://www.google.com/search?newwindow=1&sxsrf=ALeKk02W1TwJCCsa4i_6FXwlVcvAsKVYog%3A1586591871001&source=hp&ei=fniRXsjkOcer3AOulZnYAg&q=golang+%E5%AE%9E%E7%8E%B0%E6%A0%88&oq=golang+%E5%AE%9E%E7%8E%B0%E6%A0%88&gs_lcp=CgZwc3ktYWIQAzIECAAQDDIFCAAQzQI6BAgjECc6AggAOgUIABDLAUojCBcSHzQtNzg3Zzg0N2cwZzY2OGczMTlnNDE0ZzM1OWczMDNKFQgYEhE0LTFnMWcwZzFnMWcxZzZnMVCqtZ0BWODmnQFg3eedAWgCcAB4AYABygaIAaotkgELMi0yLjYuMS4yLjKYAQCgAQGqAQdnd3Mtd2l6&sclient=psy-ab&ved=0ahUKEwjIm_ym89_oAhXHFXcKHa5KBisQ4dUDCAc&uact=5)
