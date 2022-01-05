---
title: 'gomodifytags:好用的struct标签操作工具'
date: 2016-04-16 21:04:14
tags: Tools
---


<br>

[gomodifytags](https://github.com/fatih/gomodifytags)

<br>


`go get github.com/fatih/gomodifytags`



对于 `struct_demo.go`:


```go
package main

type Server struct {
	Name        string
	Port        int
	EnableLogs  bool
	BaseDomain  string
	Credentials struct {
		Username string
		Password string
	}
}
```

<br>

### <font color="#228B22">针对整个结构体进行操作</font>

<br>

`gomodifytags -file struct_demo.go(文件名) -struct Server(结构体名) -add-tags json -w`

(-w 是直接改写之前的结构体)

则会自动加上json标签:

```go
package main

type Server struct {
        Name        string `json:"name"`
        Port        int    `json:"port"`
        EnableLogs  bool   `json:"enable_logs"`
        BaseDomain  string `json:"base_domain"`
        Credentials struct {
                Username string `json:"username"`
                Password string `json:"password"`
        } `json:"credentials"`
}
```

<br>


支持3种命名样式:

- snakecase: “BaseDomain” -> “base_domain”,蛇形命名法
- camelcase: “BaseDomain” -> “baseDomain”,驼峰命名法
- lispcase: “BaseDomain” -> “base-domain”

<br>




**<font color="#4169E1">如再给该结构体添加一个驼峰格式的xml标签,但不要直接改写,输出到命令行即可:</font>**


`gomodifytags -file demo.go -struct Server -add-tags xml -transform camelcase`


```go
package main

type Server struct {
        Name        string `json:"name" xml:"name"`
        Port        int    `json:"port" xml:"port"`
        EnableLogs  bool   `json:"enable_logs" xml:"enableLogs"`
        BaseDomain  string `json:"base_domain" xml:"baseDomain"`
        Credentials struct {
                Username string `json:"username" xml:"username"`
                Password string `json:"password" xml:"password"`
        } `json:"credentials" xml:"credentials"`
}
```


<br>



**<font color="#4169E1">给结构体添加一个"validate:gt=1"和"scope:read-only"标签:</font>**

`gomodifytags -file struct_demo.go -struct Server -add-tags validate:gt=1,scope:read-only `

```go
package main

type Server struct {
        Name        string `json:"name" validate:"gt=1" scope:"read-only"`
        Port        int    `json:"port" validate:"gt=1" scope:"read-only"`
        EnableLogs  bool   `json:"enable_logs" validate:"gt=1" scope:"read-only"`
        BaseDomain  string `json:"base_domain" validate:"gt=1" scope:"read-only"`
        Credentials struct {
                Username string `json:"username" validate:"gt=1" scope:"read-only"`
                Password string `json:"password" validate:"gt=1" scope:"read-only"`
        } `json:"credentials" validate:"gt=1" scope:"read-only"`
}
```



<br>



**<font color="#4169E1">给之前的json标签,在字段名后 添加一个"omitempty",并直接改写:</font>**

`gomodifytags -file struct_demo.go -struct Server -add-tags json --add-options json=omitempty -w `

```go
package main

type Server struct {
	Name        string `json:"name,omitempty"`
	Port        int    `json:"port,omitempty"`
	EnableLogs  bool   `json:"enable_logs,omitempty"`
	BaseDomain  string `json:"base_domain,omitempty"`
	Credentials struct {
		Username string `json:"username,omitempty"`
		Password string `json:"password,omitempty"`
	} `json:"credentials,omitempty"`
}
```


<br>


`gomodifytags -file struct_demo.go -struct Server -remove-options json=omitempty `

**<font color="#4169E1">移除刚刚添加的omitempty:</font>**

```go
package main

type Server struct {
        Name        string `json:"name"`
        Port        int    `json:"port"`
        EnableLogs  bool   `json:"enable_logs"`
        BaseDomain  string `json:"base_domain"`
        Credentials struct {
                Username string `json:"username"`
                Password string `json:"password"`
        } `json:"credentials"`
}
```

<br>




**<font color="#4169E1">移除掉json标签:</font>**


`gomodifytags -file struct_demo.go -struct Server -remove-tags json `

```go
package main

type Server struct {
        Name        string 
        Port        int    
        EnableLogs  bool   
        BaseDomain  string 
        Credentials struct {
                Username string 
                Password string 
        } 
}

```

<br>

**<font color="#4169E1">移除掉所有标签:</font>**

`gomodifytags -file struct_demo.go -struct Server -clear-tags `


<br>


移除所有标签的第一个值之后的所有内容,

`gomodifytags -file struct_demo.go -struct Server -clear-options -w`

如对于

```go
package main

type Server struct {
	Name        string `json:"name,omitempty" xml:"name,attr"`
	Port        int    `json:"port,omitempty" xml:"port,attr"`
	EnableLogs  bool   `json:"enable_logs,omitempty" xml:"enable_logs,attr"`
	BaseDomain  string `json:"base_domain,omitempty" xml:"base_domain,attr"`
	Credentials struct {
		Username string `json:"username,omitempty" xml:"username,attr"`
		Password string `json:"password,omitempty" xml:"password,attr"`
	} `json:"credentials,omitempty" xml:"credentials,attr"`
}
```

执行后变为:

```go
package main

type Server struct {
	Name        string `json:"name" xml:"name"`
	Port        int    `json:"port" xml:"port"`
	EnableLogs  bool   `json:"enable_logs" xml:"enable_logs"`
	BaseDomain  string `json:"base_domain" xml:"base_domain"`
	Credentials struct {
		Username string `json:"username" xml:"username"`
		Password string `json:"password" xml:"password"`
	} `json:"credentials" xml:"credentials"`
}
```

<br>


---


<br>

### <font color="#228B22">针对结构体的某几行进行操作</font>

<br>

`gomodifytags -file struct_demo.go -line 8,11 -clear-tags json`


`gomodifytags -file struct_demo.go -line 6,7 -remove-tags json`


`gomodifytags -file struct_demo.go -line 2,7 -add-tags bson`


`gomodifytags -file struct_demo.go -offset 100 -add-tags bson`

<br>

这几个命令完全可以"望文生义",无需解释..

<br>


---


<br>

参考自 [gomodifytags](https://brantou.github.io/2017/11/07/go-modify-tags/)