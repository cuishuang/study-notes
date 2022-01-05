---
title: 谨慎使用strings.TrimLeft
date: 2016-03-09 21:56:54
tags: [Go,bug]
---

<br>

```go
package main

import (
	"fmt"
	"strings"
)

func main() {

	str := "microsoftoffice"
	rs := strings.TrimLeft(str, "microsoft")

	fmt.Println(rs)

}

```
<br>

预期输出为`office`,但实际输出为`e`

黑人问号  ???

查看strings包中TrimLeft()方法的定义,

```go
// TrimLeft returns a slice of the string s with all leading
// Unicode code points contained in cutset removed.
//
// To remove a prefix, use TrimPrefix instead.
func TrimLeft(s string, cutset string) string {
	if s == "" || cutset == "" {
		return s
	}
	return TrimLeftFunc(s, makeCutsetFunc(cutset))
}
```

即`TrimLeft`会从字符串的左边开始，找包含**待匹配字符子串中的任何最小字符**，直到找不到为止, 然后把最后找到的一个字符的左边位置的字符串全部移除,返回剩下的部分.


上例中, 从`microsoftoffice`中寻找匹配`microsoft`中任何一个最小字符( 即`m,i,c,r,o,s,f,t`)任何一个,直到找不到为止. `office`中的`o,f,i,c` 都会被匹配,最后返回匹配不到的`e`



注释中也说了,想实现预期,可使用`TrimPrefix`

> To remove a prefix, use TrimPrefix instead.

如下:

```go
package main

import (
	"fmt"
	"strings"
)

func main() {

	str := "microsoftoffice"
	rs := strings.TrimPrefix(str, "microsoft")

	fmt.Println(rs)

}
```

输出为:

```
office
```






