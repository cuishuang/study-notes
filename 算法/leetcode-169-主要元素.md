---
title: leetcode-169 主要元素
date: 2015-03-01 00:02:49
tags: 算法
---

[169. Majority Element](https://leetcode.com/problems/majority-element/)

中文版: [主要元素](https://leetcode-cn.com/problems/find-majority-element-lcci/)



---


> Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times. <br>
You may assume that the array is non-empty and the majority element always exist in the array.

### 常规做法


```go

func majorityElement(nums []int) int {

	m := make(map[int]int)

	for _, v := range nums {

		if _, ok := m[v]; ok {

			m[v] = m[v] + 1

		} else {
			m[v] = 1
		}

		if len(nums) < 2*m[v] {
			return v
		}

	}
	return 0
}
```

**时间复杂度为O(n), 引入了一个map,空间复杂度为O(n)**


---



### 进阶做法


> 你有办法在时间复杂度为 O(N)，空间复杂度为 O(1) 内完成吗？



```go


```