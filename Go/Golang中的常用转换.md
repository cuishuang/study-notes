---
title: Golang中的常用转换
date: 2019-12-02 21:23:14
tags: Go
---


### int/string互转

```go

str := strconv.Itoa(19930217) // "19930217"


i,err := strconv.Atoi("19930217") //19930217

```


<br>


### int64/string直接互转

```go
    str := strconv.FormatInt(9223372036854775808,10) //"9223372036854775808"

    i64, err := strconv.ParseInt("9223372036854775808", 10, 64)  //9223372036854775808
```


<br>


### string和float32/64 互转

```go
	f64,err := strconv.ParseFloat("3.12345678",64) //3.12345678
	f32,err := strconv.ParseFloat("3.12345678",32) //3.1234567165374756
```

<br>


```go
    s1 := strconv.FormatFloat(3.12345678, 'f', -1, 32)//float32,3.1234567
	s2 := strconv.FormatFloat(3.12345678, 'f', -1, 64)//float64,3.12345678

	s3 := strconv.FormatFloat(3.1234567, 'f', -1, 32)//float32,3.1234567
    s4 := strconv.FormatFloat(3.1234567, 'f', -1, 64)//float64,3.1234567
```