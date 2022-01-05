---
title: 'Go interface{}转struct'
date: 2019-12-21 21:29:20
tags: Go
---


写一个通用的方法，更具type字段区分不同类型，入参不同，返回值也不同



POST方式，type=8带在路径中，其他业务相关的参数放在body里

如果用gin框架，可以用`c.GetQuery(key)`获取type类型

```go
//解析query
	typstr, ok := c.GetQuery("type")
```



<br>

因为入参不同，返回值也不同，所以req和res都需要用interface{}类型，而不能是某个特定的struct

具体的方法格式如下：

`functionName(ctx context.Context, typ int, req interface{}) (res interface{}, err error)`


通过switch case，不同类型进行不同处理：

```go
switch typ {
	case 1:

		var newReq protocol.XXXXXReq
		reqByte, reqByteErr := json.Marshal(req)
		if reqByteErr != nil {
			return nil, reqByteErr
		}
		jsonRes := json.Unmarshal(reqByte, &newReq)
		if jsonRes != nil {
			return nil, jsonRes
		}
		return func1type(newReq) // 处理type=1这种类型
	default:
		return

	}

```

可见，func1type的入参是一个结构体，而原始的req是一个interface{}

如上采用了JSON序列化的方式，先将interface类型的入参json.Marshal，再将得到的byte类型的变量reqByte进行 json.Unmarshal(reqByte,&定义的结构体)

这样就实现了从interface到struct的转换。



<br>


还有一种方式，是 使用断言，做强制转换


可参考 

[Golang interface{}转struct的两种方法](https://www.mofan.life/2021/03/28/Golang-interface-%E8%BD%ACstruct%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%96%B9%E6%B3%95/)
