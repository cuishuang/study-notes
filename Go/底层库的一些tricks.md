---
title: 底层库的一些tricks
date: 2021-07-11 21:31:53
tags: Go
---



### 位运算代替乘法运算

<br>

   hash = ((hash << 5) + hash) + (*str); //等价hash(i-1) * 33 + str[i]。5(2) = 32(10)

   还有一个事情很有意思，乘以33是用左移和加法实现的。底层库对性能要求高啊。

https://zhuanlan.zhihu.com/p/37390018



[RK算法中计算 旋转hash值](https://github.com/golang/go/blob/2ca44fe2213b53ccaf6f555c11858c6e36490624/src/internal/bytealg/bytealg.go#L54)

```go
// primeRK is the prime base used in Rabin-Karp algorithm.
const primeRK = 16777619

// hashStr returns the hash and the appropriate multiplicative
// factor for use in Rabin-Karp algorithm.
func hashStr(sep string) (uint32, uint32) {
	hash := uint32(0)
	for i := 0; i < len(sep); i++ {
		hash = hash*primeRK + uint32(sep[i])
	}
	var pow, sq uint32 = 1, primeRK
	for i := len(sep); i > 0; i >>= 1 {
		if i&1 != 0 {
			pow *= sq
		}
		sq *= sq
	}
	return hash, pow
}
```

