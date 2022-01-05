---
title: 'Github的Actions,可以用来做哪些事儿'
date: 2017-10-14 21:18:14
tags: Tools
---


有很多借助Actions，实现自动签到功能的项目。

（所有放到.github/workflows文件夹里的.yml文件都会被视为Actions的配置文件）

一般操作是，先fork，然后`Settings-->Secrets`,点击右上角的`New repository secret` 添加需要的K-V配置。

而后就会按设定的频次自动执行；

当然也可以手动触发：点击`Actions`，**点allow，enable** --> **Run workflow** 进行触发。

还可以结合 [Server酱](http://sc.ftqq.com/3.version)，将执行信息推送到微信或钉钉等


<br>



如：


- [自动领取京豆](https://github.com/waslog/JD_Sign_Action)

- [有道云笔记每天自动签到](https://github.com/waslog/youdaoyunnote_sign)

<br>




更多

[白嫖Github Action学习CI/CD](https://www.bilibili.com/video/BV1244y1r7Yg)
