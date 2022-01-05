---
title: ErrGroup-有错误返回的Goroutine控制
date: 2020-10-15 21:36:13
tags: Go
categories: sync
---




```go
package main

import (
	"fmt"
	"time"
)

func main() {

	timeNow := time.Now()

	a := f1()

	b := f2()

	c := f3()

	fmt.Println("和为:", a+b+c)

	fmt.Println("耗时:", time.Since(timeNow))

}

func f1() int {

	time.Sleep(1e9)

	return 1
}

func f2() int {

	time.Sleep(1e9)

	return 2
}

func f3() int {

	time.Sleep(1e9)

	return 3
}
```

运行结果为:

```go
和为: 6
耗时: 3.00640904s
```


<br>





```go
package main

import (
	"fmt"
	"golang.org/x/sync/errgroup"
	"time"
)

func main() {

	var (
		g = &errgroup.Group{}
		a int
		b int
		c int
	)

	timeNow := time.Now()

	g.Go(func() error {
		a = f1()
		return nil
	})

	g.Go(func() error {
		b = f2()
		return nil
	})

	g.Go(func() error {
		c = f3()
		return nil
	})

	g.Wait()

	fmt.Println("和为:", a+b+c)

	fmt.Println("耗时:", time.Since(timeNow))

}

func f1() int {

	time.Sleep(1e9)

	return 1
}

func f2() int {

	time.Sleep(1e9)

	return 2
}

func f3() int {

	time.Sleep(1e9)

	return 3
}
```


运行结果为:

```go
和为: 6
耗时: 1.003517107s
```