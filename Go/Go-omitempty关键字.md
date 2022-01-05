---
title: Go omitempty关键字
date: 2017-11-13 20:42:58
tags: Go
---


### omitempty的使用

<br>

将结构体转成json作为参数，请求某个服务。希望结构体某字段为空时，解析成的json没有该字段。 这就是omitempty关键字的作用，即**忽略空值**


如：


```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID     int64  `json:"id"`
	Name   string `json:"string"`
	Gender string `json:"gender,omitempty"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"男",
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}
```

执行结果为：

```go
json of s =  {"id":15,"string":"XiaoMing","gender":"男"}
```

<br>

如果gender字段为空，即

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID     int64  `json:"id"`
	Name   string `json:"string"`
	Gender string `json:"gender,omitempty"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"",
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}
```

则执行结果为：


```go
json of s =  {"id":15,"string":"XiaoMing"}
```

<br>

而如果去掉omitempty标签，则执行结果为 

`json of s =  {"id":15,"string":"XiaoMing","gender":""}`


<br>



### omitempty的陷阱

<br>



不过该关键字有几个注意事项，或称之为"小坑"

如再增加一个address字段，为一个结构体


<br>


```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID      int64   `json:"id"`
	Name    string  `json:"string"`
	Gender  string  `json:"gender,omitempty"`
	Address address `json:"address,omitempty"`
}

type address struct {
	Country  string `json:"country"`
	Province string `json:"province"`
	City     string `json:"accessKey"`
	Street   string `json:"street"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"",
		address{},
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}

```

执行结果为：

```go
json of s =  {"id":15,"string":"XiaoMing","address":{"country":"","province":"","accessKey":"","street":""}}
```

<br>

为什么明明Address字段加了omitempty，当其为空值时，*json.Marshal*时还是有这个字段？


这是因为，**omitempty关键字无法忽略掉嵌套结构体**

<br>



那该如何解决？

可以把Address字段定义为指针类型，这样 Golang 就能知道一个指针的“空值”是多少了，否则面对一个我们自定义的结构， Golang 是猜不出我们想要的空值的...

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID      int64    `json:"id"`
	Name    string   `json:"string"`
	Gender  string   `json:"gender,omitempty"`
	Address *address `json:"address,omitempty"`
}

type address struct {
	Country  string `json:"country"`
	Province string `json:"province"`
	City     string `json:"accessKey"`
	Street   string `json:"street"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"",
		nil,
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}

```

输出为

`json of s =  {"id":15,"string":"XiaoMing"}`

<br>

但如果是

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID      int64    `json:"id"`
	Name    string   `json:"string"`
	Gender  string   `json:"gender,omitempty"`
	Address *address `json:"address,omitempty"`
}

type address struct {
	Country  string `json:"country"`
	Province string `json:"province"`
	City     string `json:"accessKey"`
	Street   string `json:"street"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"",
		&address{},
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}

```

则输出还是 

```go
json of s =  {"id":15,"string":"XiaoMing","address":{"country":"","province":"","accessKey":"","street":""}}
```


<br>

另一个陷阱，是对于用 omitempty 定义的 field ，如果给它赋的值恰好等于默认空值的话，在转为 json 之后也不会输出这个 field 



<br>


参考

[Golang 的 “omitempty” 关键字略解](https://old-panda.com/2019/12/11/golang-omitempty/)


<br>





### omitempty源码

<br>

其源码位于[src/encoding/asn1/common.go](https://gitee.com/cuishuang/go1.16/blob/master/src/encoding/asn1/common.go#L149)

```go
// Given a tag string with the format specified in the package comment,
// parseFieldParameters will parse it into a fieldParameters structure,
// ignoring unknown parts of the string.
func parseFieldParameters(str string) (ret fieldParameters) {
	var part string
	for len(str) > 0 {
		// This loop uses IndexByte and explicit slicing
		// instead of strings.Split(str, ",") to reduce allocations.
		i := strings.IndexByte(str, ',')
		if i < 0 {
			part, str = str, ""
		} else {
			part, str = str[:i], str[i+1:]
		}
		switch {
		case part == "optional":
			ret.optional = true
		case part == "explicit":
			ret.explicit = true
			if ret.tag == nil {
				ret.tag = new(int)
			}
		case part == "generalized":
			ret.timeType = TagGeneralizedTime
		case part == "utc":
			ret.timeType = TagUTCTime
		case part == "ia5":
			ret.stringType = TagIA5String
		case part == "printable":
			ret.stringType = TagPrintableString
		case part == "numeric":
			ret.stringType = TagNumericString
		case part == "utf8":
			ret.stringType = TagUTF8String
		case strings.HasPrefix(part, "default:"):
			i, err := strconv.ParseInt(part[8:], 10, 64)
			if err == nil {
				ret.defaultValue = new(int64)
				*ret.defaultValue = i
			}
		case strings.HasPrefix(part, "tag:"):
			i, err := strconv.Atoi(part[4:])
			if err == nil {
				ret.tag = new(int)
				*ret.tag = i
			}
		case part == "set":
			ret.set = true
		case part == "application":
			ret.application = true
			if ret.tag == nil {
				ret.tag = new(int)
			}
		case part == "private":
			ret.private = true
			if ret.tag == nil {
				ret.tag = new(int)
			}
		case part == "omitempty":
			ret.omitEmpty = true
		}
	}
	return
}
```



<br>



### json.Rawmessage，也许是这种场景更好的方案

<br>




另外，这种场景其实可以用`json.Rawmessage`：

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID     int64  `json:"id"`
	Name   string `json:"string"`
	Gender string `json:"gender,omitempty"`
	//Address *address `json:"address,omitempty"`
	Address json.RawMessage `json:"address,omitempty"`
}

type address struct {
	Country  string `json:"country"`
	Province string `json:"province"`
	City     string `json:"accessKey"`
	Street   string `json:"street"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"",
		nil,
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}

```

输出为：

`json of s =  {"id":15,"string":"XiaoMing"}`


<br>


### interface{}也可以

<br>

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID     int64  `json:"id"`
	Name   string `json:"string"`
	Gender string `json:"gender,omitempty"`
	//Address *address `json:"address,omitempty"`
	//Address json.RawMessage `json:"address,omitempty"`
	Address interface{} `json:"address,omitempty"`
}

type address struct {
	Country  string `json:"country"`
	Province string `json:"province"`
	City     string `json:"accessKey"`
	Street   string `json:"street"`
}

func main() {

	s := User{
		15,
		"XiaoMing",
		"",
		nil,
	}
	sJson, err := json.Marshal(s)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("json of s = ", string(sJson))

}
```

输出为：

`json of s =  {"id":15,"string":"XiaoMing"}`
