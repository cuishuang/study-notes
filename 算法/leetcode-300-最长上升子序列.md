---
title: leetcode-300 最长上升子序列
date: 2015-03-01 00:05:00
tags: 算法
---

<br>

&此题为 **动态规划** 类型的典型题目 &


[300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

难度:  <font color="orange">**中等**</font>


<img src="leetcode-300-最长上升子序列/0.png" width = 80% height = 50% />


---

### 暴力法:

<br>

<img src="leetcode-300-最长上升子序列/1.png" width = 80% height = 50% />
<img src="leetcode-300-最长上升子序列/2.png" width = 80% height = 50% />
<img src="leetcode-300-最长上升子序列/3.png" width = 80% height = 50% />

状态转移方程式,即一个递推公式

已经证明了有最优子结构,那有重复子问题吗?


<img src="leetcode-300-最长上升子序列/4.png" width = 80% height = 50% />


<br>

时间复杂度分析:

<img src="leetcode-300-最长上升子序列/5.png" width = 80% height = 50% />
<img src="leetcode-300-最长上升子序列/6.png" width = 80% height = 50% />

递归的写法需要耗费非常多的重复计算..避免重叠计算`---`一种办法就是"记忆化",把算好的保存起来

记忆化总是发生在递归之后

<img src="leetcode-300-最长上升子序列/7.png" width = 80% height = 50% />


时间复杂度分析:

<img src="leetcode-300-最长上升子序列/8.png" width = 80% height = 50% />


对于这种将问题规模不断减少的做法,称为`自顶向下`的方法

<img src="leetcode-300-最长上升子序列/9.png" width = 80% height = 50% />
<img src="leetcode-300-最长上升子序列/10.png" width = 80% height = 50% />

时间复杂度分析:

<img src="leetcode-300-最长上升子序列/11.png" width = 80% height = 50% />


<br>

---

<br>


### 动态规划解题难点:

<br>


- 应当采用什么样的数据结构,来保存什么样的计算结果?

- 如何利用保存下来的计算结果,推导出状态转移方程.

<br>

---

<br>

