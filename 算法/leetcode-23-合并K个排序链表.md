---
title: leetcode-23 合并K个排序链表
date: 2015-03-01 00:00:23
tags: 算法
---

<br>

&此题为 **高频面试题** 的典型 &



[23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

难度:  <font color="darkred">**困难**</font>

<br>



### 解法1:暴力法

用一个数组保存所有链表中的数,然后进行排序,从头到尾将数组遍历,生成一个排好序的链表;

假设每个链表的平均长度为n,则整体的时间复杂度为O(nk * log(nk))

<br>

### 解法2:最小堆

<br>

<img src="leetcode-23-合并K个排序链表/1.png" width = 90% height = 50% />

<img src="leetcode-23-合并K个排序链表/2.png" width = 90% height = 50% />

一直维护这个大小为k的最小堆


<img src="leetcode-23-合并K个排序链表/3.png" width = 90% height = 50% />

<img src="leetcode-23-合并K个排序链表/4.png" width = 90% height = 50% />



<br>

### 解法3:分治法

<br>


<img src="leetcode-23-合并K个排序链表/5.png" width = 90% height = 50% />
<img src="leetcode-23-合并K个排序链表/6.png" width = 90% height = 50% />




