---
title: 关于time.Duration()的注释
date: 2019-11-15 21:16:31
tags: Go
---


> A Duration represents the elapsed time between two instants as an int64 nanosecond count. The representation limits the largest representable duration to approximately 290 years.


*Duration 将两个瞬间之间经过的时间表示为 int64 纳秒计数。 该表示将最大可表示持续时间限制为大约 290 年。*





[src/time/time.go](https://gitee.com/cuishuang/go/blob/master/src/time/time.go#L587)
```go
// A Duration represents the elapsed time between two instants
// as an int64 nanosecond count. The representation limits the
// largest representable duration to approximately 290 years.
type Duration int64

const (
	minDuration Duration = -1 << 63
	maxDuration Duration = 1<<63 - 1
)

// Common durations. There is no definition for units of Day or larger
// to avoid confusion across daylight savings time zone transitions.
//
// To count the number of units in a Duration, divide:
//	second := time.Second
//	fmt.Print(int64(second/time.Millisecond)) // prints 1000
//
// To convert an integer number of units to a Duration, multiply:
//	seconds := 10
//	fmt.Print(time.Duration(seconds)*time.Second) // prints 10s
//
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)

```


<br>


```go
func main(){
    minDuration  := -1 << 63
	maxDuration  := 1<<63 - 1

	fmt.Println("minDuration:",minDuration)
	fmt.Println("maxDuration:",maxDuration)

}
```

输出为：

```go
minDuration: -9223372036854775808
maxDuration: 9223372036854775807
```

即可以表征从Unix时间零点，即北京时间`1970-01-01 08：00：00`，到过后*9223372036854775807*纳秒之后，这之间的时间间隔


1s= 1e9 ns

即约为*9223372036*秒

转化为北京时间，约为 `2262-04-12 07:47:16`,中间相隔292年零4个多月




