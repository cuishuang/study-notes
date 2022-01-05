---
title: Go实现一个判断版本号是否合规的函数
date: 2018-06-19 19:15:40
tags: Go
---

<br>

> 写一个判断版本号格式的方法，版本号分3部分，样例为：a1.1.1,第一部分为字母加数字，后面两部分为纯数字（支持扩展性，支持规定数字的范围）. 尽量不要使用正则匹配

<br>

```go
package main

import (
	"fmt"
	"strconv"
	"strings"
	"unicode"
)

func main() {

	str := "a12.1.1"

	intLeft2 := 0

	intRight2 := 8

	intLeft3 := 0

	intRight3 := 8

	fmt.Println("是否合规:", tool(str, intLeft2, intRight2, intLeft3, intRight3))

}

func tool(str string, intLeft2, intRight2, intLeft3, intRight3 int) bool {

	//是否是以.分割的三部分
	a := strings.Split(str, ".")

	if len(a) != 3 {
		return false
	}

	//是否第一部分为字母+数字

	var sli []int32

	firstPart := a[0]
	firstPartRuneSli := []rune(firstPart)

	//如果第一部分的首元素不为字母,直接返回false
	if ! unicode.IsLetter(firstPartRuneSli[0]) {
		return false
	}

	//如果第一部分的最后一个元素不为字母,也直接返回false
	if ! unicode.IsDigit(firstPartRuneSli[len(firstPartRuneSli)-1]) {
		return false
	}

	//保证不会字符/数字相间分布
	for _, v := range firstPart {
		//fmt.Println("v:",v)
		if ! unicode.IsLetter(v) {
			sli = append(sli, v)
		}
	}

	for _, item := range sli {
		if ! unicode.IsDigit(item) {
			return false
		}
	}

	//后两部分为数字,且范围在给定范围内

	for _, v := range a[1] {
		if ! unicode.IsDigit(v) {
			return false
		}
	}

	secondPartInt, err := strconv.Atoi(a[1])

	if err != nil {
		panic(err)
	}

	if secondPartInt < intLeft2 || secondPartInt > intRight2 {
		return false
	}

	thirdPartInt, err := strconv.Atoi(a[2])

	if err != nil {
		panic(err)
	}

	if thirdPartInt < intLeft3 || secondPartInt > intRight3 {
		return false
	}

	return true

}
```
