---
title: golang之map入门
date: 2017-12-06 19:23:03
tags: [Go,Pearl]
---

<br>



### 0.如何确定key是否存在? 如果访问了不存在的key会如何？

<br>

确定key是否存在,用**ok判别式**

```go
if _,ok := m[key]; ok {
    print("key存在")
} else {
    print("key不存在)
}
```

参考:

[ok判别式](http://www.dashen.tech/2017/07/31/ok%E5%88%A4%E5%88%AB%E5%BC%8F/)

<br>


<font color="orange"><b>
在Go中操作map,, 无论key是否存在,都不会panic或者返回error!

即可以访问不存在的key, 得到的值是对应 value 类型的零值

</b></font>


如下:


```go
package main

import "fmt"

func main() {

	m := make(map[int]int)

	m[0] = 111

	m[1] = 1313

	m[2] = 9876

    fmt.Println(m[4])
    
	m2 := make(map[string]string)

	fmt.Println(m2["cuishuang"])

	var m3 map[string]string

	fmt.Println(m3["dashen"])

}
```

输出为:

```go
0


```



<br>


---



### 1.key的类型可以是哪些

<br>

简言之,可以判断(==)的类型,都可以作为map的键名.

如 bool, 数字，string, 指针, channel , 还有 只包含前面几个类型的 interface , struct, array; 

显然，slice， map 以及 function 是不可以的，因为这几种类型无法用 == 来判断;

原文如下:<br>

> As mentioned earlier(如前所述), map keys may be of any type that is comparable. The language spec defines this precisely, but in short, comparable types are boolean, numeric, string, pointer, channel, and interface types, and structs or arrays that contain only those types. Notably absent from the list are slices, maps, and functions; these types cannot be compared using ==, and may not be used as map keys.

<b> comparable types</b>, 即可比较类型,可判等类型

---


### 2.map键值互换

<br>


`  o := map[string]int{"a": 0, "b": 0, "c": 2} `

如果原map的值不唯一如上，会导致新map无法完全包含原map的键值对。该问题可以采用多值map来解决,即将新map的键值定义为一个切片类型


```go

var (
    o = map[string]int{"a": 0, "b": 0, "c": 2}
)

func main() {

    fmt.Println(o)

    n := map[int][]string{}
    for k, v := range o {
        n[v] = append(n[v], k)
    }
    fmt.Println(n)
}
```

输出:
```
map[a:0 b:0 c:2]
map[0:[a b] 2:[c]]
```

---


### 3.可以对map的键值取指针吗?

<br>

> 不可以!

`map 中的元素并不是一个变量，而是一个值。因此，我们不能对 map 的元素进行取址操作。`


```go
package main

import (
	"fmt"
)

func main() {
	
	m := make(map[string]int)
	
	m["age"] = 27
	
	fmt.Println(&m["age"])
}
```

报错:

```go
cannot take the address of m["age"]
```


### 4.当 map 的键值为结构体类型的值，那么无法直接修改结构体中的字段值

<br>

https://blog.csdn.net/k346k346/article/details/90729484


<br>

---


### 5.map之间能否直接判等?

<br>

不可以!

直接将使用 **map1 == map2** 是错误的。

map后面直接用 `==` 比较,只能用来判断 map 是否为 nil, 即只可以 "m == nil"


可以通过`reflect.DeepEqual`来"深度比较"两个slice/struct/map 是否相等

参考:

[如何比较两个 map 相等](https://github.com/qcrao/Go-Questions/blob/master/map/%E5%A6%82%E4%BD%95%E6%AF%94%E8%BE%83%E4%B8%A4%E4%B8%AA%20map%20%E7%9B%B8%E7%AD%89.md)

[比较两个 slice/struct/map 是否相等](https://mozillazg.com/2014/11/go-compare-struct-slice-map-is-equal.html)

<br>


---

<br>

### 6.实现key有序的map

<br>

//TODO
https://juejin.im/entry/5bbd9282e51d450ea246cd43


### 7.map 的并发读写问题

<br>

参见:

[golang之map并发访问](http://www.dashen.tech/2019/01/18/golang%E4%B9%8Bmap%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE/)

---
