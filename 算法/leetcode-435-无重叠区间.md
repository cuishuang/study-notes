---
title: leetcode-435 无重叠区间
date: 2015-03-01 00:07:15
tags: 算法
---

<br>

&此题为 **高频试题-合并区间** 类型的典型题目 &



[435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

难度:  <font color="orange">**中等**</font>


<img src="leetcode-435-无重叠区间/0.png" width = 80% height = 50% />

---


<br>


### 方法1:暴力法

<br>


<img src="leetcode-435-无重叠区间/1.png" width = 80% height = 50% />


<img src="leetcode-435-无重叠区间/2.png" width = 80% height = 50% />

因为n*(2的n-1次方)远大于n*log(n),故而排序的时间复杂度就可以忽略不计了


<br>


### 方法2:暴力法2

<br>

<img src="leetcode-435-无重叠区间/3.png" width = 80% height = 50% />

<img src="leetcode-435-无重叠区间/4.png" width = 80% height = 50% />


<br>


### 方法3:贪婪法1

<br>


<img src="leetcode-435-无重叠区间/5.png" width = 80% height = 50% />



<br>


### 方法4:贪婪法2

<br>


按照结束时间排序

<img src="leetcode-435-无重叠区间/6.png" width = 80% height = 50% />

<img src="leetcode-435-无重叠区间/7.png" width = 80% height = 50% />


