---
title: leetcode-407 接雨水II
date: 2015-03-01 00:06:47
tags: 算法
---

<br>

&此题为 **难题-接雨水II** 类型的典型 &



[407. 接雨水 II](https://leetcode-cn.com/problems/trapping-rain-water-ii/)


难度:  <font color="darkred">**困难**</font>

<br>

---



<img src="leetcode-407-接雨水II/0.png" width = 90% height = 50% />

<br>  


从里面向外寻找,

对于每一个点,都要不断地往外去寻找超过自己,又最矮的柱子


时间复杂度为O(n的立方)

<img src="leetcode-407-接雨水II/1.png" width = 90% height = 50% />

<br>



采用农村包围城市的做法,从外面往里面寻找

从最外面最矮的那个开始,慢慢向里面计算(做BFS,即广度优先算法)

为什么不先选择最高的那个呢?因为决定接雨水的高度,是由最矮的那个决定的~

怎样可以快速知道,接下来哪个高度最矮呢?  可以借用有限队列提高速度

<br>

代码如下:


<img src="leetcode-407-接雨水II/2.png" width = 90% height = 50% />
<img src="leetcode-407-接雨水II/3.png" width = 90% height = 50% />




<img src="leetcode-407-接雨水II/4.png" width = 90% height = 50% />
<img src="leetcode-407-接雨水II/5.png" width = 90% height = 50% />



<br>


**复杂度分析:**

<br>

<img src="leetcode-407-接雨水II/6.png" width = 90% height = 50% />




[leetcode-417 太平洋大西洋水流问题]()