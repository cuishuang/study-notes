---
title: spew--极其好用的Go语言深层打印包
date: 2016-12-30 18:39:08
tags: Go
---

<br>

### 深度打印

<br>

如果变量为接口/多层嵌套的复杂结构体,且里面包含指针类型,用fmt打印时,结果将十分不可读. [go-spew](https://github.com/davecgh/go-spew)这个package,非常完美地解决了此问题.




<font size=1 color="skyblue">
spew为动词,译作"(使) 喷出，涌出",也有"呕吐"的意思.读作"si biu".

[例句]The volcano spewed out more scorching volcanic ashes, gases and rocks
火山喷出更多灼热的火山灰、气体和岩块。


</font>

```go
package main

import (
	"fmt"
	"github.com/davecgh/go-spew/spew"
)

type AppDeveloper struct {
	DeveloperID string   `json:"id"`
	Name        *string  `json:"name"`
	Email       string   `json:"email"`
	CompanyInfo *company `json:"company"`
}

type company struct {
	companyName  string
	companyOwner *string
	address      string
	isListed     *bool
}

func main() {

	name := "Alex"
	entrepreneur := "马化腾" //企业家,创业者
	isPubliced := true    //香港和英国：a public company; 美国：a listed company

	user := AppDeveloper{
		"123",
		&name, "i@dashen.tech",
		&company{
			"腾讯",
			&entrepreneur,
			"深圳",
			&isPubliced},
	}

	fmt.Println(user)

	n, err := spew.Println(user)

	fmt.Println(n, err)

}

```

输出为:

```result
{123 0xc0000102d0 i@dashen.tech 0xc000066390}

{123 <*>Alex i@dashen.tech <*>{腾讯 <*>马化腾 深圳 <*>true}}

68 <nil>

```

<br>

> // Println is a wrapper for fmt.Println that treats each argument as if it were
 passed with a default Formatter interface returned by NewFormatter.  It
 returns the number of bytes written and any write error encountered.  See
 NewFormatter for formatting details.

> spew.Println() 返回写入的字节数以及遇到的任何写入错误

可见,如果遇到pointer类型,会在前面标出为<*>类型,但输出的还是该指针在内存里存放的值; 

和fmt相比,可读性强太多了

--- 

### spew包中其他的方法

<br>

##### spew.Dump:

```go
package main

import (
	"github.com/davecgh/go-spew/spew"
)

type Project struct {
	Id      int64   `json:"project_id"`
	Title   string  `json:"title"`
	Name    string  `json:"name"`
	Data    *string `json:"data"`
	Commits *string `json:"commits"`
}

func main() {

	commit := "第一次提交"
	o := Project{Name: "hello", Title: "world", Commits: &commit}
	spew.Dump(o)
}
```
<br>

输出为:

```result
(main.Project) {
 Id: (int64) 0,
 Title: (string) (len=5) "world",
 Name: (string) (len=5) "hello",
 Data: (*string)(<nil>),
 Commits: (*string)(0xc0000682b0)((len=15) "第一次提交")
}
```
<br>


##### spew.Printf:

```go
package main

import (
	"fmt"
	"github.com/davecgh/go-spew/spew"
)

func main() {

	ui8 := uint8(5)
	pui8 := &ui8
	ppui8 := &pui8

	// Create a circular data type.
	type circular struct {
		ui8 uint8
		c   *circular
	}
	c := circular{ui8: 1}
	c.c = &c

	fmt.Println("pui8:", pui8)
	fmt.Println("pui8:", *pui8)
	fmt.Println("------------")
	fmt.Println("ppui8:", ppui8)
	fmt.Println("ppui8:", **ppui8)

	fmt.Println("------------")

	// Print!
	spew.Printf("ppui8: %v\n", ppui8)
	spew.Printf("circular: %v\n", c)
}

```

<br>

输出为:

```result
pui8: 0xc00008e160
pui8: 5
------------
ppui8: 0xc000088030
ppui8: 5
------------
ppui8: <**>5
circular: {1 <*>{1 <*><shown>}}
```

<br>


##### spew.ConfigState:


```go
package main

import (
	"fmt"
	"github.com/davecgh/go-spew/spew"
)

func main() {

	scs := spew.ConfigState{Indent: "\t"}

	// Output using the ConfigState instance.
	v := map[string]int{"one": 1}

	scs.Printf("v: %v\n", v)

	fmt.Println("--------------------")

	scs.Dump(v)
}
```

<br>

输出为:

```result
v: map[one:1]
--------------------
(map[string]int) (len=1) {
        (string) (len=3) "one": (int) 1
}
```


> // Indent specifies the string to use for each indentation level.  The
	 global config instance that all top-level functions use set this to a
	 single space by default.  If you would like more indentation, you might
	 set this to a tab with "\t" or perhaps two spaces with "  ".

> //缩进指定用于每个缩进级别的字符串。 默认情况下，所有顶级功能使用的全局配置实例将其设置为单个空间。 如果您希望缩进更多，可以将其设置为带有“ \ t”的选项卡，或者带有“”的两个空格。

<br>


---

<br>

### 番外: `*`符号有两个用处:

1. 表明这是一个指针类型,是一个状态,形容词;<br>
2. 解引用,是一个动作,动词

如 上面代码中的Name字段的类型为*string,即要求该字段需要是字符串指针类型.

变量name为`string`类型,则取指针即`&name`后即变为`*string`类型,对`&name`解引用`*&name`则又变为`string`类型


```go
package main

import "fmt"

func main() {
	name := "fliter"
	fmt.Println(*&name)

}
```

输出为:

```result
fliter
```


---


[更多可点击](https://blog.csdn.net/weixin_42580403/article/details/86763272)
