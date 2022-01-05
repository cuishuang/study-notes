---
title: leetcode-10 正则表达式匹配
date: 2015-03-01 00:00:10
tags: 算法
---


<br>

&此题为 **难题-正则表达式匹配** 类型的典型 &



[10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

难度:  <font color="darkred">**困难**</font>

<br>



<img src="leetcode-10-正则表达式匹配/0.png" width = 90% height = 50% />


经常使用正则匹配,但很少思考其背后如何实现...

在此只需要实现正则表达式里的两个小功能


<br>


---


<img src="leetcode-10-正则表达式匹配/1.png" width = 90% height = 50% />

<br>

### 递归解法1:

从前往后递归调用:

<img src="leetcode-10-正则表达式匹配/2.png" width = 90% height = 50% />
<img src="leetcode-10-正则表达式匹配/3.png" width = 90% height = 50% />
<img src="leetcode-10-正则表达式匹配/4.png" width = 90% height = 50% />

<br>


### 递归解法2:

从后往前递归调用:


<img src="leetcode-10-正则表达式匹配/5.png" width = 90% height = 50% />
<img src="leetcode-10-正则表达式匹配/6.png" width = 90% height = 50% />
<img src="leetcode-10-正则表达式匹配/7.png" width = 90% height = 50% />
<img src="leetcode-10-正则表达式匹配/8.png" width = 90% height = 50% />



<br>

---

<br>

一道思路相似的题目:

[leetcode-44 通配符匹配]()