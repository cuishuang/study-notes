---
title: King's Day Speech
date: 2016-06-17 19:49:12
tags: 译海一粟
---


原文作者[*Guido van Rossum*](https://www.blogger.com/profile/12821714508588242516), 地址[King's Day Speech](https://neopythonic.blogspot.com/2016/04/kings-day-speech.html),仅作翻译及注解



<br>


Today the Netherlands celebrates King's Day. To honor this tradition, the Dutch embassy in San Francisco invited me to give a "TED talk" to an audience of Dutch and American entrepreneurs. Here's the text I read to them. Part of it is the tl;dr of my autobiography; part of it is about the significance of programming languages; part of it is about Python's big idea. Leve de koning! (Long live the king!)

<br>

<font size=1 color="#008080">

注: 荷兰前100多年都是女王，因此一直称女王节。威廉王子2014年即位后，变身国王节。&nbsp;
女王节固定为4月30日，原为庆祝威廉王子曾曾祖母的生日。国王节为威廉王子的生日4月27日。&nbsp;
节日当天国王会发表祝词并巡游。&nbsp;
当天的热闹程度堪比嘉年华，全国大街小巷全是地摊，每户人家把闲置的东西以极便宜的价格售出。

[全“橙”狂欢，荷兰 King’s Day国王节](https://zhuanlan.zhihu.com/p/67522522)

Dutch embassy

荷兰大使馆

autobiography 

n. 自传；自传文学

</font>





<font size=4>Python: a programming language created by a community</font>

<br>


Excuse my ramblings. I’ll get to a point eventually.

Let me introduce myself. I’m a nerd, a geek. I’m probably somewhere on the autism spectrum. I‘m also a late bloomer. I graduated from college when I was 26. I was 45 when I got married. I’m now 60 years old, with a 14 year old son. Maybe I just have a hard time with decisions: I’ve lived in the US for over 20 years and I am still a permanent resident.

I'm no Steve Jobs or Mark Zuckerberg. But at age 35 I created a programming language that got a bit of a following. What happened next was pretty amazing. But I'll get to that.

At age 10 my parents gave me an educational electronics kit. The kit was made by Philips, and it was amazing. At first I just followed the directions and everything worked; later I figured out how to design my own circuits. My prized possessions were the kit's three (!) transistors.



<font size=1 color="#008080">

ramblings  

n. 漫无边际的讲话，东拉西扯的讲话；胡扯表不满;  随笔


late bloomer 

开花晚的植物；智力发展晚的人. 大器晚成的人


permanent resident   

永久性居民


circuits 

n. [电子] 电路（circuit的复数）；环路；巡回
v. 环行；巡回旅行（circuit的三单形式）

prized  

adj. 珍贵的，宝贵的

possessions 

n. [经] 财产；所有物（possession的复数形式）


worldly possessions: 身外之物


transistors

n. [电子] 晶体管；晶体三极管（transistor的复数）

</font>

I took one of my first electronics models, a blinking light, to show and tell in 5th grade. It was a total dud — nobody cared or understood its importance. I think that's one of my earliest memories of finding myself a geek: until then I had just been a quiet quick learner.

In high school I developed my nerdiness further — I hung out with a few other kids interested in electronics, and during physics class we sat in the back of the class discussing NAND gates while the rest of the class was still figuring out Ohm's law.

Fortunately our physics teacher had figured us out: he employed us to build a digital timer that he used to demonstrate the law of gravity to the rest of the class. It was a great project and showed us that our skills were useful. The other kids still thought we were weird: it was the seventies and many were into smoking pot and rebelling; another group was already preparing for successful careers as doctors or lawyers or tech managers. But they left me alone, I left them alone, and I graduated as one of the best of my year.

After high school I went to the University of Amsterdam: It was close to home, and to a teen growing up in the Netherlands in the seventies, Amsterdam was the only cool city. (Yes, the student protests of 1968 did touch me a bit.) Much to my high school physics teacher's surprise and disappointment, I chose to major in math, not physics. But looking back I think it didn’t matter.

<font size=1 color="#008080">

dud 

n. 衣服；[军] 哑弹；无用物

adj. 无用的


NAND gates 与非; 与非门

figured out: 想出;理解;弄清

demonstrate 

vt. 证明；展示；论证
vi. 示威


weird  

adj. 怪异的；不可思议的；超自然的


rebelling   

叛逆,反抗

Rebelling Teenage: 叛逆的青春期孩子


Much to my high school physics teacher's surprise and disappointment, I chose to major in math, not physics. But looking back I think it didn’t matter.

让我的高中物理老师感到惊讶和失望的是，我选择了数学专业，而不是物理专业。 但是回想一下，我认为这没关系。

</font>




In the basement of the science building was a mainframe computer, and it was love at first sight. Card punches! Line printers! Batch jobs! More to the point, I quickly learned to program, in languages with names like Algol, Fortran and Pascal. Mostly forgotten names, but highly influential at the time. Soon I was, again, sitting in the back of class, ignoring the lecture, correcting my computer programs. And why was that?

In that basement, around the mainframe, something amazing was happening. There was a loosely-knit group of students and staff with similar interests, and we exchanged tricks of the trade. We shared subroutines and programs. We united in our alliances against the mainframe staff, especially in the endless cat-and-mouse games over disk space. (Disk space was precious in a way you cannot understand today.)

But the most important lesson I learned was about sharing: while most of the programming tricks I learned there died with the mainframe era, the idea that software needs to be shared is stronger than ever. Today we call it open source, and it’s a movement. Hold that thought!

At the time, my immediate knowledge of the tricks and the trade seemed to matter most though. The mainframe’s operating system group employed a few part-time students, and when they posted a vacancy, I applied, and got the job. **It was a life-changing event!** Suddenly I had unlimited access to the mainframe — no more fighting for space or terminals — plus access to the source code for its operating system, and dozens of colleagues who showed me how all that stuff worked.



<font size=1 color="#008080">

basement 

n. 地下室；地窖

mainframe computer  

大型计算机；主机电脑

it was love at first sight. 一见钟情

card punch  [计] 卡片穿孔机

Line printers 行式打印机

Batch jobs 批处理作业

loosely knit
组织松散的, adj. having only distant social or legal ties

Yet sometimes the hype is justified, in particular in the case of open- source software, free programs developed by loosely knit groups of developers. 

特别是由分散各地的开发者紧密结合在一起免费程序——开源软件。

endless adj. 无止境的；连续的；环状的；漫无目的的

endless: 无止境的

Endless Love: 无尽的爱

Endless Space: 无尽空间


vacancy  

n. 空缺；空位；空白；空虚


</font>

I now had my dream job, programming all day, with real customers: other programmers, the users of the mainframe. I stalled my studies and essentially dropped out of college, and I would not have graduated if not for my enlightened manager and a professor who hadn't given up on me. They nudged me towards finishing some classes and pulled some strings, and eventually, with much delay, I did graduate. Yay!

I immediately *landed* a new dream job that would not have been open to me without that degree. I had never lost my interest in programming languages as an object of study, and I joined a team building a new programming language — not something you see every day. The designers hoped their language would take over the world, replacing Basic.

It was the eighties now, and Basic was the language of choice for a new generation of amateur programmers, coding on microcomputers like the Apple II and the Commodore 64. Our team considered the Basic language a pest that the world should be rid of. The language we were building, ABC, would "stamp out Basic", according to our motto.

Sadly, for a variety of reasons, our marketing (or perhaps our timing) sucked, and after four years, ABC was abandoned. Since then I've spent many hours trying to understand why the project failed, despite its heart being so clearly in the right place. Apart from being somewhat over-engineered, my best answer is that ABC died because there was no internet in those days, and as a result there could not be a healthy feedback loop between the makers of the language and its users. ABC’s design was essentially a one-way street.


<font size=1 color="#008080">


Stalled: 止步不前

stalled train: 停滞列车

drop out  

退出；退学；脱离

enlightened  

adj. 开明的；文明的；有知识的；觉悟的

nudged   

v. 推进；轻推；用肘轻推；唠叨；往前挤；鼓励；接近（nudge 的过去式和过去分词）


landed 

v. 降落；（使）飞机平稳着陆；（乘飞机或船）着陆；（使）着陆；跳落（land 的过去式及过去分词）
adj. 拥有大量土地的；包括大量土地的；

take over the world 席卷全球

pest 

n. 害虫；有害之物；讨厌的人

stamp out  

扑灭；踩灭. 此处译为 淘汰 比较贴切

over-engineered 过度设计



</font>



Just half a decade later, when I was picking through ABC’s ashes looking for ideas for my own language, that missing feedback loop was one of the things I decided to improve upon. “Release early, release often” became my motto (freely after the old Chicago Democrats’ encouragement, “vote early, vote often”). And the internet, small and slow as it was in 1990, made it possible.

Looking back 25 years, the Internet and the Open Source movement (a.k.a. Free Software) really did change everything. Plus something called Moore's Law, which makes computers faster every year. Together, these have entirely changed the interaction between the makers and users of computer software. It is my belief that these developments (and how I managed to make good use of them) have contributed more to the success of “my” programming language than my programming skills and experience, no matter how awesome.

It also didn't hurt that I named my language Python. This was a bit of unwitting marketing genius on my part. I meant to honor the irreverent comedic genius of Monty Python's Flying Circus, and back in 1990 I didn't think I had much to lose. Nowadays, I'm sure "brand research" firms would be happy to charge you a very large fee to tell you exactly what complex of associations this name tickles in the subconscious of the typical customer. But I was just being flippant.

I have promised the ambassador not to bore you with a technical discussion of the merits of different programming languages. But I would like to say a few things about what programming languages mean to the people who use them — programmers. Typically when you ask a programmer to explain to a lay person what a programming language is, they will say that it is how you tell a computer what to do. But if that was all, why would they be so passionate about programming languages when they talk among themselves?


<font size=1 color="#008080">

Just half a decade later  仅仅五年后


Looking back 25 years 回顾25年


interaction 

n. 相互作用，相互影响；交流；[数] 交互作用；互动

unwitting  

adj. 不知情的；不知不觉的，无意的


subconscious 

adj. 潜意识的，下意识的

n. 下意识（心理活动），潜意识（心理活动）


flippant 

adj. 轻率的；嘴碎的；没礼貌的


ambassador  

n. 大使；代表；使节


Typically 通常

lay person 门外汉, 外行


passionate 

adj. 热情的；热烈的，激昂的；易怒的

</font>


In reality, programming languages are how programmers express and communicate ideas — and the audience for those ideas is other programmers, not computers. The reason: the computer can take care of itself, but programmers are always working with other programmers, and poorly communicated ideas can cause expensive flops. In fact, ideas expressed in a programming language also often reach the end users of the program — people who will never read or even know about the program, but who nevertheless are affected by it.

Think of the incredible success of companies like Google or Facebook. At the core of these are ideas — ideas about what computers can do for people. To be effective, an idea must be expressed as a computer program, using a programming language. The language that is best to express an idea will give the team using that language a key advantage, because it gives the team members — people! — clarity about that idea. The ideas underlying Google and Facebook couldn't be more different, and indeed these companies' favorite programming languages are at opposite ends of the spectrum of programming language design. And that’s exactly my point.

True story: The first version of Google was written in Python. The reason: Python was the right language to express the original ideas that Larry Page and Sergey Brin had about how to index the web and organize search results. And they could run their ideas on a computer, too!

So, in 1990, long before Google and Facebook, I made my own programming language, and named it Python. But what is the idea of Python? Why is it so successful? How does Python distinguish itself from other programming languages? (Why are you all staring at me like that? :-)


<font size=1 color="#008080">


In reality / In fact  实际上

clarity  

n. 清楚，明晰；透明

spectrum 

n. 光谱；频谱；范围；余象


And that’s exactly my point. 这正是我的意思。

</font>




I have many answers, some quite technical, some from my specific skills and experience at the time, *some just about being in the right place at the right time*. But I believe the most important idea is that Python is developed on the Internet, entirely in the open, by a community of volunteers (but not amateurs!) *who feel passion and ownership*.

And that is what that group of geeks in the basement of the science building was all about.

Surprise: Like any good inspirational speech, the point of this talk is about happiness!

I am happiest when I feel that I'm part of such a community. I’m lucky that I can feel it in my day job too. (I'm a principal engineer at Dropbox.) If I can't feel it, I don't feel alive. And so it is for the other community members. The feeling is contagious, and there are members of our community all over the world.


<font size=1 color="#008080">


inspirational

adj. 鼓舞人心的；带有灵感的，给予灵感的

principal engineer 首席工程师

principal  

adj. 主要的；资本的
n. 首长；校长；资本；当事人

contagious  

adj. 感染性的；会蔓延的; 有感染力的

</font>


The Python user community is formed of millions of people who consciously use Python, and love using it. There are active members organizing Python conferences — affectionately known as PyCons — in faraway places like Namibia, Iran, Iraq, even Ohio!

My favorite story: A year ago I spent 20 minutes on a video conference call with a classroom full of faculty and staff at Babylon University in southern Iraq, answering questions about Python. Thanks to the efforts of the audacious woman who organized this event in a war-ridden country, students at Babylon University are now being taught introductory programming classes using Python. I still tear up when I think about the power of that experience. In my wildest dreams I never expected I’d touch lives so far away and so different from my own.

And on that note I'd like to leave you: a programming language created by a community fosters happiness in its users around the world. Next year I may go to PyCon Cuba!


<font size=1 color="#008080">

consciously 

adv. 自觉地；有意识地


affectionately  

adv. 亲切地；挚爱地



faculty and staff  n. 教职员,教职工


audacious  

adj. 无畏的；鲁莽的


war-ridden	战火纷飞; 兵连祸结;


In my wildest dreams  在我最狂野的梦想中


fosters  

v. 培养，促进；代养，照料（他人子女一段时间）（foster 的第三人称单数）


</font>
