---
title: leetcode-22 括号生成
date: 2015-03-01 00:00:22
tags: 算法
---

<br>

&此题为使用  **栈/ Stack** 求解的典型&


[22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

难度:  <font color="orange">**中等**</font>



<img src="leetcode-22-括号生成/1.png" width = 80% height = 50% />


<br>



> 利用一个栈,不断地往里面压左括号,一旦遇到一个右括号,就把栈顶的左括号弹出来,表示这是一个合法的组合..直到最后,判断栈里还有无左括号剩余