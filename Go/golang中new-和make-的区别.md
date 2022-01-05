---
title: golang中new()和make()的区别
date: 2017-06-18 11:18:56
tags: [Go,Interview]
---

<br>

* make 只适用于3种内建的引用类型：切片、map 和 channel。make(T) 返回一个类型为 T 的初始值，不是指针

* new(T) 返回类型为*T; 即new(某个结构体),得到的结果是该结构体的指针; new 适用于值类型,如数组和结构体；new(T)相当于&T{}


**new一般不常用,而用&T{}代替**


---

### 小问题: 如下输出为true还是false?


```go
package main

import "fmt"

func main() {

	type LogParamter struct {
		//name string
	}

	p1 := LogParamter{}
	p2 := &LogParamter{}
	p3 := new(LogParamter)

	fmt.Println(p1)
	fmt.Printf("%p\n", p2)
	fmt.Printf("%p\n", p3)

	fmt.Println(p2 == p3)

}
```


当`LogParamter`为空结构体时,输出为:

```go
{}
0x1196a78
0x1196a78
true
```

当`LogParamter`为

```go
type LogParamter struct {
		name string
    }
```
时,输出为:

```go
{}
0xc0000661e0
0xc0000661f0
false
```



如果不比较指针而直接比较值, 若`LogParamter`中某个字段的类型不能判等,即如:

```go
package main

import "fmt"

func main() {

	type LogParamter struct {
		//name string
		info map[string]string
	}

	p1 := LogParamter{}

	// p2 := &LogParamter{}
	// p3 := new(LogParamter)

	// fmt.Println(p1)
	// fmt.Printf("%p\n", p2)
	// fmt.Printf("%p\n", p3)
	// fmt.Println(p2 == p3)
	
	
	p4 := LogParamter{}
	fmt.Println(p1 == p4) //Invalid operation: p1 == p4 (operator == not defined on LogParamter)

}
```

则直接编译不通过

<br>


只有在结构体的所有字段类型全部支持判等时，才可做(值)判断操作





