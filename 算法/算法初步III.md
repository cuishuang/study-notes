---
title: 算法初步III
date: 2015-02-01 00:00:02
tags: 算法
---


<br>



## <font color="red">8.剖析大厂算法面试真题-高频题精讲(一)</font>


<br>

<img src="算法初步III/8-1.png" width = 80% height = 50% />



---

<br>

#### <font color="darkgreen">3.无重复字符的最长子串</font>


[leetcode-3 最大字符子串](http://www.dashen.tech/2015/03/01/leetcode-3-%E6%9C%80%E5%A4%A7%E5%AD%97%E7%AC%A6%E5%AD%90%E4%B8%B2/)


**Longest Common SubString, 又称为"LCS"问题**


<br>




1. 暴力法:

<img src="算法初步III/8-2.png" width = 80% height = 50% />
<img src="算法初步III/8-3.png" width = 80% height = 50% />


<br>


**子序列**和**子串**: 子序列不需要连续

<img src="算法初步III/8-4.png" width = 80% height = 50% />


暴力法的时间复杂度:

<img src="算法初步III/8-5.png" width = 80% height = 50% />


---

<br>

2. 线性法:


<img src="算法初步III/8-6.png" width = 80% height = 50% />
<img src="算法初步III/8-7.png" width = 80% height = 50% />


<br>


**复杂度分析:**

<img src="算法初步III/8-8.png" width = 80% height = 50% />


<img src="算法初步III/8-9.png" width = 80% height = 50% />


<br>

---


3. 优化的线性法:


<img src="算法初步III/8-10.png" width = 80% height = 50% />



---

<br>


#### <font color="darkgreen">4.寻找两个有序数组的中位数</font>


[leetcode-4 寻找两个有序数组的中位数](http://www.dashen.tech/2015/03/01/leetcode-4-%E5%AF%BB%E6%89%BE%E4%B8%A4%E4%B8%AA%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E4%B8%AD%E4%BD%8D%E6%95%B0/)



<br>


1. 暴力法:


<img src="算法初步III/8-11.png" width = 80% height = 50% />

<br>

---


2. 切分法:


<img src="算法初步III/8-12.png" width = 80% height = 50% />

<img src="算法初步III/8-13.png" width = 80% height = 50% />
<img src="算法初步III/8-14.png" width = 80% height = 50% />
<img src="算法初步III/8-15.png" width = 80% height = 50% />
<img src="算法初步III/8-16.png" width = 80% height = 50% />
<img src="算法初步III/8-17.png" width = 80% height = 50% />


<img src="算法初步III/8-18.png" width = 80% height = 50% />
<img src="算法初步III/8-19.png" width = 80% height = 50% />
<img src="算法初步III/8-20.png" width = 80% height = 50% />


<br>

*时间复杂度:*

<img src="算法初步III/8-21.png" width = 80% height = 50% />



**扩展一:**

如果给定的两个数组都是没有经过排序处理的,应该如何找出中位数呢?


先合并排序,再找出中位数.

<img src="算法初步III/8-22.png" width = 80% height = 50% />


有无更好的方法呢?

<img src="算法初步III/8-23.png" width = 80% height = 50% />



<img src="算法初步III/8-24.png" width = 80% height = 50% />


<img src="算法初步III/8-25.png" width = 80% height = 50% />

<br>

**扩展二:**

<img src="算法初步III/8-26.png" width = 80% height = 50% />

对于分布式的大数据处理,应考虑两方面的限制:

- 每台服务器进行算法计算的复杂度限制,包括时间和空间复杂度
 - 空间复杂度: 假设存储的都是32位整型,即4个字节,那么10亿个数需占用40亿字节,大约40GB
 - 而快速排序的空间复杂度为log(n),那大约30次堆栈压入



- 服务器与服务器之间进行通信时的网络带宽限制


<img src="算法初步III/8-27.png" width = 80% height = 50% />


<br>



经典的快速选择算法


<br>


[leetcode-23 合并K个排序链表](http://www.dashen.tech/2015/03/01/leetcode-23-%E5%90%88%E5%B9%B6K%E4%B8%AA%E6%8E%92%E5%BA%8F%E9%93%BE%E8%A1%A8/)


---



## <font color="darkgreen">8.剖析大厂算法面试真题-高频题精讲(二)</font>


<br>

<img src="算法初步III/9-1.png" width = 80% height = 50% />

- 合并区间+无重叠区间

- 火星字典

- 基本计算器

<br>

---

<br>

#### <font color="darkgreen">56.合并区间</font>

<br>

[leetcode-56 合并区间](http://www.dashen.tech/2015/03/01/leetcode-56-%E5%90%88%E5%B9%B6%E5%8C%BA%E9%97%B4/)

<br>

#### <font color="darkgreen">435.无重叠区间</font>

<br>

[leetcode-435 无重叠区间](http://www.dashen.tech/2015/03/01/leetcode-435-%E6%97%A0%E9%87%8D%E5%8F%A0%E5%8C%BA%E9%97%B4/)

<br>

力扣上还有很多关于`区间`的问题


<br>

---

<br>

#### <font color="darkgreen">269.火星字典</font>

<br>

[leetcode-269 火星字典](http://www.dashen.tech/2015/03/01/leetcode-269-%E7%81%AB%E6%98%9F%E5%AD%97%E5%85%B8/)



<br>

---

<br>

#### <font color="darkgreen">772.基本计算器III</font>

<br>

[leetcode-772 基本计算器III](http://www.dashen.tech/2015/03/01/leetcode-772-%E5%9F%BA%E6%9C%AC%E8%AE%A1%E7%AE%97%E5%99%A8III/)




<br>

---



## <font color="red">10.剖析大厂算法面试真题-难题精讲(一)</font>


<br>

<img src="算法初步III/10-1.png" width = 80% height = 50% />

<br>


- 正则表达式匹配

- 柱状图中的最大矩形

- 实现strStr()

---

<br>

#### <font color="darkgreen">10.正则表达式匹配</font>


<br>

[leetcode-10 正则表达式匹配](http://www.dashen.tech/2015/03/01/leetcode-10-%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%8C%B9%E9%85%8D/)


[leetcode-44 通配符匹配]()




<br>

#### <font color="darkgreen">84.柱状图中最大的矩形</font>

<br>

[leetcode-84 柱状图中最大的矩形]()


<br>

#### <font color="darkgreen">28.实现strStr()</font>
<br>

[leetcode-28 实现strStr()]()





<br>

---



## <font color="red">11.剖析大厂算法面试真题-难题精讲(二)</font>


<br>

<img src="算法初步III/11-1.png" width = 80% height = 50% />

<br>


- 回文对

- 至多包含K个不同字符的最长子串

- 接雨水II


---

<br>

#### <font color="darkgreen">336.回文对</font>


<br>

[leetcode-336 回文对](http://www.dashen.tech/2015/03/01/leetcode-336-%E5%9B%9E%E6%96%87%E5%AF%B9/)


<br>

#### <font color="darkgreen">340.至多包含K个不同字符的最长子串</font>

<br>

[leetcode-340 至多包含K个不同字符的最长子串](http://www.dashen.tech/2015/03/01/leetcode-340-%E8%87%B3%E5%A4%9A%E5%8C%85%E5%90%ABK%E4%B8%AA%E4%B8%8D%E5%90%8C%E5%AD%97%E7%AC%A6%E7%9A%84%E6%9C%80%E9%95%BF%E5%AD%90%E4%B8%B2/)



<br>


#### <font color="darkgreen">407.接雨水II</font>

<br>

[leetcode-407 接雨水II](http://www.dashen.tech/2015/03/01/leetcode-407-%E6%8E%A5%E9%9B%A8%E6%B0%B4II/)

[leetcode-417 太平洋大西洋水流问题]()


<br>

---



## <font color="red">12.冲刺</font>


<br>

<img src="算法初步III/12-1.png" width = 80% height = 50% />

<br>

- 刷题
- 简历
- 面试经验


---

<br>

#### <font color="darkgreen">刷题/Leetcode</font>

<br>


<img src="算法初步III/12-2.png" width = 80% height = 50% />

这点上Golang差好多,能直接用的只有slice,map这聊聊几个...


最关键的不是量的问题,而是质的问题


<img src="算法初步III/12-3.png" width = 80% height = 50% />

<img src="算法初步III/12-4.png" width = 80% height = 50% />

<br>

*白板面试注意事项:*

<img src="算法初步III/12-5.png" width = 80% height = 50% />

<br>

**刷过的题目忘了怎么办?**

<img src="算法初步III/12-6.png" width = 80% height = 50% />

<br>



#### <font color="darkgreen">简历/Resume</font>

<br>

<img src="算法初步III/12-7.png" width = 80% height = 50% />


<br>



#### <font color="darkgreen">面试经验/Interview</font>

<br>


<img src="算法初步III/12-8.png" width = 80% height = 50% />

<img src="算法初步III/12-9.png" width = 80% height = 50% />