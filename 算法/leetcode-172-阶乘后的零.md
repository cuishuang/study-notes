---
title: leetcode-172 阶乘后的零
date: 2015-03-01 00:02:52
tags: 算法
---



[172. 阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

难度:  <font color="green">**简单**</font>


<img src="leetcode-172-阶乘后的零/0.png" width = 90% height = 50% />

<br>

题目与 [面试题 16.05. 阶乘尾数](https://leetcode-cn.com/problems/factorial-zeros-lcci/) 完全一致


---

<br>


```go
func trailingZeroes(n int) int {
        if n/5 == 0 {
		return 0
	}
	return n/5 + trailingZeroes(n/5)

}

```