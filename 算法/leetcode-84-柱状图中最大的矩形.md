---
title: leetcode-84 柱状图中最大的矩形
date: 2015-03-01 00:01:24
tags: 算法
---

<br>

&此题为 **难题-柱状图中最大的矩形** 类型的典型 &



[84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)


难度:  <font color="darkred">**困难**</font>

<br>

---



<img src="leetcode-84-柱状图中最大的矩形/0.png" width = 90% height = 50% />

<br>  


### 暴力法:

<br>




<img src="leetcode-84-柱状图中最大的矩形/1.png" width = 90% height = 50% />

时间复杂度为O(n的平方);

---



当遇到一个下降的高度时,就可以开始计算

遇到一个上升的高度,不着急计算




<img src="leetcode-84-柱状图中最大的矩形/2.png" width = 90% height = 50% />

**复杂度分析:**

<img src="leetcode-84-柱状图中最大的矩形/3.png" width = 90% height = 50% />

<img src="leetcode-84-柱状图中最大的矩形/4.png" width = 90% height = 50% />


