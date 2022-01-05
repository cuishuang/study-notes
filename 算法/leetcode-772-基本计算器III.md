---
title: leetcode-772 基本计算器III
date: 2015-03-01 00:12:52
tags: 算法
---


<br>

&此题为 **高频面试题-基本计算器** 类型的典型 &



[772. 基本计算器III]()

难度:  <font color="darkred">**困难**</font>

<br>



<img src="leetcode-772-基本计算器III/0.png" width = 90% height = 50% />



---



<img src="leetcode-772-基本计算器III/1.png" width = 90% height = 50% />
<img src="leetcode-772-基本计算器III/2.png" width = 90% height = 50% />

<img src="leetcode-772-基本计算器III/3.png" width = 90% height = 50% />



**升级一下:支持减法**

1. 借助两个stack

2. 将减号看做是加"负数"


<img src="leetcode-772-基本计算器III/4.png" width = 90% height = 50% />
<img src="leetcode-772-基本计算器III/5.png" width = 90% height = 50% />


<br>


**继续升级:支持乘法和除法**

- 需要考虑符号的优先级

最开始堆栈为空



<img src="leetcode-772-基本计算器III/6.png" width = 90% height = 50% />
<img src="leetcode-772-基本计算器III/7.png" width = 90% height = 50% />



<br>


**继续升级:支持小括号**



<img src="leetcode-772-基本计算器III/8.png" width = 90% height = 50% />


<img src="leetcode-772-基本计算器III/9.png" width = 90% height = 50% />
<img src="leetcode-772-基本计算器III/10.png" width = 90% height = 50% />
<img src="leetcode-772-基本计算器III/11.png" width = 90% height = 50% />


