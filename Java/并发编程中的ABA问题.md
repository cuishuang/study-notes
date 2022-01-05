---
title: 并发编程中的ABA问题
date: 2017-08-21 22:13:20
tags: Java
categories: sync
---


*ABA* 表面看起来没什么问题，但如果结合实际应用场景，就可以看出其问题所在

<img src="并发编程中的ABA问题/0.png" width = 100% height = 100% />



真正要做到严谨的CAS机制，我们在Compare阶段不仅要比较期望值A和地址V中的实际值，还要比较变量的版本号是否一致。



<br>




[漫画：什么是CAS机制？（进阶篇）](https://mp.weixin.qq.com/s/nRnQKhiSUrDKu3mz3vItWg)







[风险指针(Hazard Pointer)](https://www.google.com.hk/search?q=Hazard%E6%8C%87%E9%92%88&oq=Hazard%E6%8C%87%E9%92%88&aqs=chrome..69i57j69i60.163j0j4&sourceid=chrome&ie=UTF-8),

也称 *冒险指针*

<img src="并发编程中的ABA问题/1.png" width = 100% height = 100% />
