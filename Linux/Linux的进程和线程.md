---
title: Linux的进程和线程
date: 2016-11-04 18:41:09
tags: Linux
---


<br>

点击 


[Linux内核分析与应用3-进程管理](https://dashen.tech/2020/05/17/Linux%E5%86%85%E6%A0%B8%E5%88%86%E6%9E%90%E4%B8%8E%E5%BA%94%E7%94%A83-%E8%BF%9B%E7%A8%8B%E7%AE%A1%E7%90%86/)



[多进程与多线程](https://dashen.tech/2020/07/04/%E5%A4%9A%E8%BF%9B%E7%A8%8B%E4%B8%8E%E5%A4%9A%E7%BA%BF%E7%A8%8B/)


---


<br>

<font color="#FF1493">**fork**</font>: &emsp;&emsp;&nbsp;&emsp;&emsp;&emsp;&emsp;创建进程


<font color="#FF1493">**pthread_create**</font>: &emsp;创建线程


<font color="#DDA0DD">**ps -aux | wc -l**</font>: &emsp;&nbsp;&nbsp;统计进程数


在Linux内核中,对于线程实现非常特殊, 他并不区分线程和进程, 线程只是一种特殊的进程罢了

[Linux内核初探:进程与线程](https://juejin.im/post/6844904003508109326)

<br>



---


<font color="#DEB887">

进程有独立的地址空间,线程则没有独立地址空间. 同一个进程的不同线程, 它们之间共享地址空间的.

对于多进程模型, 因为之间相互独立, 其优点就是安全性比较好. 一个进程的crash, 不会导致整个软件的崩溃;而线程则不行,一个进程里的某个线程crash,会影响整个进程.

多进程的缺点, 是其创建和上下文切换的开销比较大, 另外进程之间要想相互通讯需要专门的机制([IPC](https://dashen.tech/2020/06/16/Linux%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1-IPC-%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F/))才能实现进程间通讯.


这恰恰是线程的优点, 如果是同一个进程的不同线程, 它们在一个进程的地址空间里，所以, 其相互通讯比较方便, 它们之间的切换也比较简单. (创建线程比较简单, 切换也比较轻巧, 通讯也比较方便, 相互协作比较好).

但多线程也有缺点,就是不稳定.因为同一个进程里的若干个线程，如果有一个(线程)崩溃, 就会使得整个进程的其他线程也一起崩溃, 相互干扰比较大. (相互通讯比较容易,相互干扰也比较大)

那软件设计时,一般采用多进程模型还是多线程模型呢? 要看具体的应用场景, 比如对于浏览器软件, (浏览器的每一个选项卡, 或说每一个浏览器页面, 是用多线程还是多进程实现更好呢?) 显然是多进程，为什么呢, 因为浏览器页面之间几乎没什么通讯需求, 所以这时候线程易于通讯的优点就发挥不出来, 反而是一个线程崩溃,导致同进程其他线程崩溃这个缺点非常致命. (肯定不希望一个页面崩溃,会连带导致其他页面也崩溃) . 所以我们一般是用多进程来实现浏览器的 ,实际上是访问同一个网站的若干页面是在一个进程的不同线程(如打开了三个新浪的新闻页,这三个页面对应一个进程), 但访问不同的网站是不同的进程(如打开了两个新浪,三个搜狐,一个网易,对应三个进程)

</font>


<br>


---

<br>

更多参阅:

[Linux 进程必知必会](https://note.youdao.com/web/#/file/recent/note/C9D10B0ED9254311A1F1FFD53CD45063/?search=%E8%BF%9B%E7%A8%8B)


[进程、线程、轻量级进程、协程和go中的Goroutine](https://note.youdao.com/web/#/file/recent/note/3DF4EE32A56A43409D7773793299A6C7/?search=%E8%BF%9B%E7%A8%8B)

[阮一峰-进程与线程的一个简单解释](https://note.youdao.com/web/#/file/recent/note/ABA00D6581324B2DB66A30B389D3B39E/?search=%E8%BF%9B%E7%A8%8B)



[Linux的任督二脉：进程调度和内存管理](https://note.youdao.com/web/#/file/recent/note/51F2B36320D14E29A6B87152778F60C6/?search=%E8%BF%9B%E7%A8%8B)


[面试必问：进程和线程有什么区别？](https://note.youdao.com/web/#/file/recent/note/4BF74D8480904997883FFA417110CD35/?search=%E8%BF%9B%E7%A8%8B)

---


