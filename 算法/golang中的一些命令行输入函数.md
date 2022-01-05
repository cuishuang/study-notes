---
title: golang中的一些命令行输入函数
date: 2018-06-20 23:34:44
tags: [Go,算法]
---

<br>


最近在刷[Nowcoder](https://www.nowcoder.com/),和leetcode不同的是,在这个平台上的提交答案,需要从接收入参开始,而不像力扣上只需要完成函数即可. 

故而开此篇,整理总结一下Golang中的一些输入函数

<br>


### <font color="#8B2252"><b>Scan,Scanln和Scanf</b></font>

<br>

```go
package main

import "fmt"

//从控制台接收用户信息
func main() {

	var name string
	var age int
	var isVip bool

	fmt.Println("请输入姓名: ")

	n, err := fmt.Scanln(&name)//会停止继续执行,直至用户输入

	fmt.Println(n, err)

	fmt.Println("请输入年龄: ")

	_, _ = fmt.Scanln(&age)//会停止继续执行,直至用户输入

	fmt.Println("是否为会员: ")

	_, _ = fmt.Scanln(&isVip)//会停止继续执行,直至用户输入

	fmt.Printf("姓名是:%s, 年龄:%d,是否为会员:%t", name, age, isVip)

}
```

<br>


关于:

```go
// Scan scans text read from standard input, storing successive
// space-separated values into successive arguments. Newlines count
// as space. It returns the number of items successfully scanned.
// If that is less than the number of arguments, err will report why.

//Scan会扫描从标准输入读取的文本，并将连续的以空格分隔的值存储到连续的参数中。 换行符算作空格。 它返回成功扫描的项目数。

//如果该数目少于参数数目，则err将报告原因。

如果该数目少于参数数目，则err将报告原因。
func Scan(a ...interface{}) (n int, err error) {
	return Fscan(os.Stdin, a...)
}

// Scanln is similar to Scan, but stops scanning at a newline and
// after the final item there must be a newline or EOF.
func Scanln(a ...interface{}) (n int, err error) {
	return Fscanln(os.Stdin, a...)
}

// Scanf scans text read from standard input, storing successive
// space-separated values into successive arguments as determined by
// the format. It returns the number of items successfully scanned.
// If that is less than the number of arguments, err will report why.
// Newlines in the input must match newlines in the format.
// The one exception: the verb %c always scans the next rune in the
// input, even if it is a space (or tab etc.) or newline.
func Scanf(format string, a ...interface{}) (n int, err error) {
	return Fscanf(os.Stdin, format, a...)
}
```

<br>


---

### <font color="#8B2252"><b>bufio.NewReader(os.Stdin) 和 ReadLine</b></font>

<br>


```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {

	reader := bufio.NewReader(os.Stdin)

	data, _, _ := reader.ReadLine()

	content := string(data)

	fmt.Println(content)

}
```

会原封不动得到在控制台里输入的内容





