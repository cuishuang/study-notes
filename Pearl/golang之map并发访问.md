---
title: golang之map并发访问
date: 2019-01-18 21:13:55
tags: [Go,Pearl]
categories: Tech
---

<br>

### map不是并发安全的!

<br>

并发安全也称线程安全,如果在并发中出现了数据的丢失或错乱,就是并发不安全

golang的map不是并发安全的，并发对map读写可能会报"fatal error"

<br>

```go
package main

import (
	"fmt"
	"time"
)

const N = 5

func main() {

	m := make(map[int]int)

	go func() {
		for i := 0; i < N; i++ {
			m[i] = i*10 + 6
		}
	}()

	go func() {
		for i := 0; i < N; i++ {
			fmt.Println(i, m[i])
		}
	}()

	time.Sleep(1e9 * 5)

}

```

输出为:

```result
0 6
1 16
2 26
3 36
4 46
```



上面第一个协程对map写,第二个对map读,

注:如上代码中,调换两个协程的顺序,并不会对结果有影响

<br>

当 N 较大如等于1000时,该程序将报错:

---


```
fatal error: concurrent map read and map write

goroutine 18 [running]:
runtime.throw(0x10cb62d, 0x21)
	/usr/local/go/src/runtime/panic.go:617 +0x72 fp=0xc00002ef30 sp=0xc00002ef00 pc=0x10281e2
runtime.mapaccess1_fast64(0x10ab340, 0xc000084000, 0x0, 0x0)
	/usr/local/go/src/runtime/map_fast64.go:21 +0x1b3 fp=0xc00002ef58 sp=0xc00002ef30 pc=0x100eac3
main.main.func2(0xc000084000)
	/Users/shuangcui/go/note/map/1.go:22 +0x67 fp=0xc00002efd8 sp=0xc00002ef58 pc=0x1093c37
runtime.goexit()
	/usr/local/go/src/runtime/asm_amd64.s:1337 +0x1 fp=0xc00002efe0 sp=0xc00002efd8 pc=0x1051b51
created by main.main
	/Users/shuangcui/go/note/map/1.go:20 +0x6a

goroutine 1 [sleep]:
runtime.goparkunlock(...)
	/usr/local/go/src/runtime/proc.go:307
time.Sleep(0x12a05f200)
	/usr/local/go/src/runtime/time.go:105 +0x159
main.main()
	/Users/shuangcui/go/note/map/1.go:26 +0x7d

goroutine 17 [runnable]:
main.main.func1(0xc000084000)
	/Users/shuangcui/go/note/map/1.go:16 +0x45
created by main.main
	/Users/shuangcui/go/note/map/1.go:14 +0x48
exit status 2
```

---

解决方案:


#### 加锁实现

<br>

可以用sync.Mutex (互斥锁)，或者sync.RWMutex(读写锁)来"重构"map,

即定义一个新的结构体,为该结构体实现两个方法,在方法里使用sync包的Mutex或RWMutex来做加锁保证并发安全.

<br>


```go
package main

import (
	"fmt"
	"time"
	"sync"
)

const N = 1000

type Cmap struct {
	m    map[int]int
	lock *sync.RWMutex
}

func (c *Cmap) Get(key int) int {
	c.lock.RLock()
	defer c.lock.RUnlock()
	return c.m[key]
}

func (c *Cmap) Set(key, val int) {
	c.lock.Lock()
	defer c.lock.Unlock()
	c.m[key] = val*10 + 8
}

func main() {

	m := make(map[int]int)

	lock := new(sync.RWMutex)

	cm := Cmap{
		m:    m,
		lock: lock,
	}

	go func() {
		for i := 0; i < N; i++ {
			fmt.Println(i, cm.Get(i))
		}
	}()

	go func() {
		for i := 0; i < N; i++ {
			cm.Set(i, i)
		}
	}()

	time.Sleep(1e9 * 5)

}
```

---

<br>

#### 直接使用sync.Map

<br>

从 go1.9 开始,新增了sync.Map,对并发情况下map的安全做了支持,可以对上面代码进行简化:

```go
package main

import (
	"sync"
	"fmt"
	"time"
)

const N = 1000

func main() {
	var m sync.Map

	go func() {
		for i := 0; i < N; i++ {
			v, _ := m.Load(i) //读
			fmt.Println(i, v)
		}
	}()

	go func() {
		for i := 0; i < N; i++ {
			m.Store(i, i*10+4) //写
		}
	}()

	time.Sleep(5 * 1e9)

}
```


<br>

Store为写,Load为读

除去Store和Load两个方法,sync.Map还有几个方法:

<br>

- 删除

> func (m *Map) Delete(key interface{})

- 遍历，类似于js的forEach循环

> func (m *Map) Range(f func(key, value interface{}) bool)

- 存或者取

> func (m *Map) LoadOrStore(key, value interface{}) (actual interface{}, loaded bool)
这个方法有点绕，举例说下：

```go
package main

import (
	"fmt"
	"sync"
)

func main() {

	var m sync.Map

	actual, loaded := m.LoadOrStore("k1", "v1")
	fmt.Println(actual, loaded) // v1 false

	actual, loaded = m.LoadOrStore("k1", "v1")
	fmt.Println(actual, loaded) // v1 true

	actual, loaded = m.LoadOrStore("k1", "v2")
	fmt.Println(actual, loaded) // v1 true

	actual, loaded = m.LoadOrStore("k2", "v2")
	fmt.Println(actual, loaded) // v2 false

}

```
<br>


LoadOrStore方法其实是先取，
如果有key的话就返回，没有再存储，即优先取，没有再存储；

该方法有两个返回值,第一个是键值,第二个是之前有无该键名(bool类型)

(和缓存的做法很类似：先从缓存中取，如果没有则从数据库中读，读回来存入缓存，下次就可以从缓存中取到了。)
