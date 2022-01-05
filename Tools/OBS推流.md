---
title: OBS推流
date: 2021-12-14 20:00:58
tags: Tools
---



为深入理解rtmp，推流，互动直播，录制，特意尝试了下使用OBS来推流到(各大)直播平台。。

<br>

以斗鱼为例，申请[成为主播后](https://mp.douyu.com/live/main)，会得到**rtmp地址**和**直播码**，



<img src="OBS推流/0.png" width = 80% height = 50% />


下载OBS


<img src="OBS推流/1.png" width = 80% height = 50% />


右下角，设置-推流-自定义:


<img src="OBS推流/2.png" width = 80% height = 50% />



<br>

接着设置来源,即想要在直播平台上显示的东西


点击加号，选择 *显示器采集*， （之前版本坑叫*显示捕获*），这样整个屏幕都会被推到直播平台



点击右侧*开始推流*，就会将选定的”来源“（此处即显示器屏幕），推流到斗鱼的房间


<img src="OBS推流/3.png" width = 80% height = 50% />






使用*窗口采集*，则只会推这个窗口的信息


<img src="OBS推流/4.png" width = 80% height = 50% />


<br>

<img src="OBS推流/5.jpeg" width = 80% height = 50% />


<br>

还需要再解决下窗口显示不全的问题






<br>


更多参考：

[OBS推流直播快速上手教程](https://www.bilibili.com/read/cv5380911/)


