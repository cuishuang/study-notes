---
title: 使用FFmpeg将视频转换成音频
date: 2021-07-02 21:32:51
tags: Tools
---


整理移动硬盘,发现了一段2017年,在西安回民街青旅,素昧平生的三人闲谈,当时为视频录制,时长近一小时40分钟,超过10G.

听了后感觉很有意思,但没必要使用视频,音频形式空间小,更合适. 

<br>

(三人分别为: 作为合伙人兼旅店日常理事的东北青年A,一表人才, 但其健谈程度远不及另外两位; 

在此无偿打工&免费住宿的与我一般大小的青年B,川陕之交的汉中宁强人,在海南读大学; 

[结束第一份工作](https://dashen.tech/2017/04/13/coming-going/), "无房车压力,有过万存款"的C. [游历古都](https://dashen.tech/2017/05/05/%E9%95%BF%E5%AE%89%E5%8F%A4%E6%84%8F/),[攀登高岳](https://dashen.tech/2017/05/30/%E5%8D%8E%E5%B1%B1%E5%88%AB%E4%BC%A0/), 便从汴州到杭州,开启了一段996生涯


 后半段适逢在天津大学读研的俄罗斯西西伯利亚留学生问路华山,和其交谈些许)

<!-- <img src="使用FFmpeg将视频转换成音频/3.png" width = 90% height = 50% /> -->


![](http://qvnyxkfve.hd-bkt.clouddn.com/ffmpeg3)


苦于本地没有视频转音频工具,和同事闲聊时,说"不就是用FFmpeg一行命令的事吗",豁然开朗.


<br>

### 安装


<br>

使用 `brew install ffmpeg` 时,因为依赖过多,(尤其升级Big Sur后),中途可能会报错:



<!-- <img src="使用FFmpeg将视频转换成音频/1.png" width = 90% height = 50% /> -->


![](http://qvnyxkfve.hd-bkt.clouddn.com/ffmpeg1)


这时仅需 `brew install 安装失败的依赖名称`, 而后再 `brew install ffmpeg`. 如此往复便可安装成功.



<br>


### 将视频转换为音频

<br>

`ffmpeg -i 视频名.MOV -vn  -acodec libmp3lame -ac 2 -qscale:a 4 -ar 48000  想要转成的音频名.mp3`


不消几分钟,便可转换成功

<!-- <img src="使用FFmpeg将视频转换成音频/2.png" width = 90% height = 50% /> -->

![](http://qvnyxkfve.hd-bkt.clouddn.com/ffmpeg2)




<br>


### 将音频切分成多段

<br>



需要对音频资源进行裁剪,同样一行命令搞定:


`ffmpeg  -i source.mp3  -vn -acodec copy -ss 00:00:00.00 -t 00:30:00 part1.mp3`


-  -ss 从 小时：分：秒 处开始切割 

-  -t 持续时间

-  -to 到 `小时：分：秒.毫秒` 处截止

<br>

<!-- <img src="使用FFmpeg将视频转换成音频/4.png" width = 90% height = 50% /> -->

![](http://qvnyxkfve.hd-bkt.clouddn.com/ffmpeg4)

<br>



### 将音频转为文字

<br>

音频内容太长,想要转成文字. 目前有很多 提供在线音频转文字 功能的平台,但大多需要收费,或体验不佳.

多番比选尝试,发现 [网易见外](https://jianwai.youdao.com/index/0) 综合下来最佳




<!-- <img src="使用FFmpeg将视频转换成音频/6.png" width = 90% height = 50% /> -->


![](http://qvnyxkfve.hd-bkt.clouddn.com/ffmpeg6)

<br>



那一年,我24岁,相当健谈,天南海北地和人闲聊. 从东北到海南,安康和汉中,天水与兰州,丽江与大理,西西伯利亚和叶尼塞河, 似是都曾去过居住多年.

几天旅途很快结束，我匆匆回归为一名打工人角色。此后几年，也曾在疲乏和劳累间隙,找寻片刻之机踏上旅途,但再未和陌生人,有如此契阔的闲谈.

<br>


> 少年侠气，交结五都雄。肝胆洞，毛发耸。立谈中，死生同。一诺千金重。推翘勇，矜豪纵。轻盖拥，联飞鞚，斗城东。轰饮酒垆，春色浮寒瓮，吸海垂虹。闲呼鹰嗾犬，白羽摘雕弓，狡穴俄空。乐匆匆。<br>
似黄粱梦，辞丹凤；明月共，漾孤蓬。官冗从，怀倥偬；落尘笼，簿书丛。鹖弁如云众，供粗用，忽奇功。笳鼓动，渔阳弄，思悲翁。不请长缨，系取天骄种，剑吼西风。恨登山临水，手寄七弦桐，目送归鸿。