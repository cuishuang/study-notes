---
title: leetcode-1 两数之和
date: 2015-03-01 00:00:01
tags: 算法
---

<br>



<img src="leetcode-1两数之和/1.png" width = 90% height = 50% />

难度:  <font color="orange">**中等**</font>



 <br>
 
 
 >最朴素的一种解法,两次循环~时间复杂度为O(n的平方)
 
 直接贴代码:
 
 ```go
 func twoSum(nums []int, target int) []int {
  
     rs := make([]int,0)
     for i,v := range nums {
         for i2,v2 := range nums{
             if v + v2 == target && i != i2 {
                 rs = append(rs,i,i2)
                 return rs
             }     
         }
     }
     return rs
 }
 ```

 <br>
<img src="leetcode-1两数之和/2.png" width = 90% height = 50% />
 <br>

***

>进一步优化时间复杂度,用空间换时间
两次一维循环,时间复杂度为O(n)

```go
func twoSum(nums []int, target int) []int {
	hash := make(map[int]int)

	for k, v := range nums {
		hash[v] = k
	}

	for k, v := range nums {
		tmp := target - v

		if _, ok := hash[tmp]; ok {
			if hash[tmp] == k {
				continue//"你不能重复利用这个数组中同样的元素",防止如目标值为6,数组为[3,2,4]这种情况下[0,0]也符合条件
			}
			return []int{k, hash[tmp]}

		}
	}

	return nil

}

```
 <br>
<img src="leetcode-1两数之和/3.png" width = 90% height = 50% />
 <br>
***

>还可以继续优化为一次一维循环,时间复杂度为O(n)

```go
func twoSum(nums []int, target int) []int {
	hash := make(map[int]int)

	for k,v := range nums {
		if j,ok := hash[target-v];ok {
			return []int{k,j}

		}

		hash[v] = k
	}


	return nil

}


```
 <br>
<img src="leetcode-1两数之和/4.png" width = 90% height = 50% />
