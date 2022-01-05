---
title: robfig/cron使用小结
date: 2017-02-22 20:30:02
tags: [Go,Tools]
---


[robfig/cron/v3](https://github.com/robfig/cron) 是一个 Golang 的定时任务库，支持 cron 表达式。其源码值得一读, 低耦合高内聚体现地淋漓尽致,其中涉及的装饰器模式,并发处理等都很值得学习。




如果是 `0 30 * * * *`, 那就是每个 xx:00 和 xx:30 执行


而如果是`@every 10m`, 则是从当前时间起(服务启动时间)，每30分钟执行一次，未必是xx:00，xx:30时刻执行



参考[一不留神就掉坑](https://dashen.tech/2021/03/01/%E4%B8%80%E4%B8%8D%E7%95%99%E7%A5%9E%E5%B0%B1%E6%8E%89%E5%9D%91/#crontab%E9%85%8D%E7%BD%AE%E9%97%AE%E9%A2%98), 可利用在线工具查看具体执行时间

<br>

---

<br>


其相关源码,可参考:


<br>

[Golang 定时任务 github/robfig/cron/v3 使用与源码解析](https://blog.csdn.net/zjbyough/article/details/113853582)

[Golang crontab定时执行任务（github.com/robfig/cron）](https://blog.csdn.net/suiban7403/article/details/78226618)

[cron表达式解析 + robfig/cron 源码剖析](https://blog.csdn.net/cchd0001/article/details/51076922)




