---
title: leetcode-28 实现strStr()
date: 2015-03-01 00:00:28
tags: 算法
---

<br>

&此题为 **难题-实现strStr()** 类型的典型 &



[28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

难度:  <font color="green">**简单**</font>

<br>



<img src="leetcode-28-实现strStr/0.png" width = 90% height = 50% />


<br>

---

<br>


### 暴力法:

比较简单


<img src="leetcode-28-实现strStr/1.png" width = 90% height = 50% />
<img src="leetcode-28-实现strStr/2.png" width = 90% height = 50% />

<br>



### KMP算法

<br>


<br>

<img src="leetcode-28-实现strStr/3.png" width = 90% height = 50% />


两个疑惑:

<img src="leetcode-28-实现strStr/4.png" width = 90% height = 50% />


KMP中重要的数据结构:

<img src="leetcode-28-实现strStr/5.png" width = 90% height = 50% />
<img src="leetcode-28-实现strStr/6.png" width = 90% height = 50% />
<img src="leetcode-28-实现strStr/7.png" width = 90% height = 50% />



<font color="gold">LPS</font>

<br>

为什么不需要比较前面的位置?

反证法可以证明: 没有必要!


当我们知道两个绿色的方块,就是最大的公共前缀和后缀时,就可以放心地进行**跳跃操作**,而不需要担心我们会担心完全匹配的情况发生.因为完美匹配不可能在跳跃的空间内发生



<img src="leetcode-28-实现strStr/8.png" width = 90% height = 50% />


<br>




<img src="leetcode-28-实现strStr/9.png" width = 90% height = 50% />
<img src="leetcode-28-实现strStr/10.png" width = 90% height = 50% />



如何求出needle字符串的最长公共前缀和后缀数组?


1. 暴力法:


<br>

<img src="leetcode-28-实现strStr/11.png" width = 90% height = 50% />


O(n的平方)



<br>



2. 更高效的做法:

<br>

O(n)



<img src="leetcode-28-实现strStr/12.png" width = 90% height = 50% />



实例:

needle = ADCADB

LPS = 000000


<img src="leetcode-28-实现strStr/13.png" width = 90% height = 50% />
<img src="leetcode-28-实现strStr/14.png" width = 90% height = 50% />



**复杂度分析:**


<img src="leetcode-28-实现strStr/15.png" width = 90% height = 50% />


KMP的代码十分精妙~


