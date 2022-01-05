---
title: Rust vs Go
date: 2020-08-15 21:41:41
tags: 译海一粟
---



原文作者*BITFIELD CONSULTING*, 地址[Rust vs Go](https://bitfieldconsulting.com/golang/rust-vs-go),仅作翻译及注解

<br>

<img src="Rust-vs-Go/rust-vs-go-wide.png" width = 100% height = 50% />



<br>



Which is better, Rust or Go? Which language should you choose for your next project, and why? How do the two compare in areas like performance, simplicity, safety, features, scale, and concurrency? What do they have in common, and where do they fundamentally differ? Let's find out, in this friendly and even-handed comparison of Rust and Golang, from the author of the [For the Love of Go](https://bitfieldconsulting.com/books/) book series.


‘For the Love of Go’ is a series of fun, easy-to-follow ebooks introducing software engineering in Go.


<font size=1 color="#008080">




even-handed 

adj. 平坦的；平稳的；相等的；均衡的；偶数的；同样大小的；平分的；平局的；镇静的


comparison

n. 比较；对照；比喻；比较关系


easy-to-follow

adj. 容易的；舒适的
adv. 不费力地，从容地


</font>

<br>


## Rust and Go are both awesome

<br>

First, it's really important to say that *both Go and Rust are absolutely excellent programming languages*. They're modern, powerful, widely-adopted, and offer excellent performance. You may have read articles and blog posts aiming to convince you that Go is better than Rust, or vice versa. But that really makes no sense; every programming language represents a set of trade-offs. Each language is optimised for different things, so your choice of language should be determined by what suits you and the problems you want to solve with it.

<br>

<font size=1 color="#008080">

awesome

adj. 令人敬畏的；使人畏惧的；可怕的；极好的



widely-adopted

adj.被广泛采用的



aim to

vt. 计划，打算；目标在于…；以…为目标


convince

vt. 说服；使确信，使信服



or vice versa

或相反,反之亦然



 makes no sense

 毫无意义



 optimised

 最佳


</font>

<br>

In this article, I'll try to give a brief overview of where I think Go is the ideal choice, and where I think Rust is a better alternative. Ideally, though, you should have a working familiarity with both languages. While they're very different in syntax and style, both Rust and Go are first-class tools for building software. With that said, let's take a closer look at the two languages.


<br>

<font size=1 color="#008080">

ideal

adj. 理想的；完美的；想象的；不切实际的

n. 理想；典范


take a closer look

仔细看看

</font>


<br>


## The similarities

<br>

Rust and Go have a lot in common, which is one reason you often hear them mentioned together. What are some of the common goals of both languages?


> Rust is a low-level statically-typed multi-paradigm programming language that’s focused on safety and performance.
—> [Gints Dreimanis](https://serokell.io/blog/rust-guide)



> Go is an open source programming language that makes it easy to build simple, reliable, and efficient software.
—> [Golang.org](https://golang.org/)


<font size=1 color="#008080">

low-level statically-typed multi-paradigm

低级 静态类型 多范式


</font>

<br>

### Memory safety

<br>


Both Go and Rust belong to the group of modern programming languages whose priority is *memory safety*. It's become clear over many decades of using older languages such as C and C++ that one of the biggest causes of bugs and security vulnerabilities is accessing memory unsafely or incorrectly. Rust and Go deal with this problem in different ways, but both aim to be smarter and safer than other languages about managing memory, and to help you write correct and performant programs.



<br>

<font size=1 color="#008080">

vulnerabilities

n. 缺陷（vulnerability 的复数形式）；脆弱点


accessing memory unsafely or incorrectly

不安全或不正确地访问内存

</font>

<br>

### Fast, compact executables

<br>


They're both compiled languages, which means your programs are translated directly to executable machine code, so that you can deploy your program as a single binary file; unlike interpreted languages such as Python and Ruby, you don't need to distribute an interpreter and lots of libraries and dependencies along with your program, which is a big plus. This also makes both Rust and Go programs extremely fast in comparison to interpreted languages.


<br>

<font size=1 color="#008080">

compiled languages

编译型语言



interpreted languages

解释型语言



executable

adj. 可执行的；可实行的



interpreter

n. 解释者；口译者；注释器


plus

n. 优势；加号；加法；附加额



in comparison to

与…相比

</font>

<br>


### General-purpose languages

<br>


Rust and Go are also both powerful, scalable general-purpose programming languages, which you can use to develop all kinds of modern software, from web applications to distributed microservices, or from embedded microcontrollers to mobile apps. Both have excellent standard libraries and a thriving third-party ecosystem, as well as great commercial support and a large user base. They've both been around for many years, and will continue to be widely used for years to come. Learning either Go or Rust today will be a sound investment of your time and effort.



<br>



<font size=1 color="#008080">

scalable

可扩展的


embedded microcontrollers

嵌入式微控制器


thriving

v. 兴旺发达；喜欢（thrive 的现在分词）

adj. 欣欣向荣的，兴旺发达的



commercial support

商务支持



sound, 居然有这么多意思

a sound investment 

明智的投资

</font>

<br>


### Pragmatic programming style

<br>


Neither are primarily functional languages (like Scala or Elixir, for example), and neither are exclusively object-oriented (like Java and C#). Instead, while both Go and Rust have features associated with functional and object-oriented programming, they're pragmatic languages aimed at solving problems in whatever way is most appropriate, rather than forcing you into a particular way of doing things. (If you like the functional style of programming, though, you’ll find a lot more facilities for it in Rust, because Rust has a lot more facilities than Go in general.)

> We can debate about what an ‘object-oriented’ language is, but it’s fair to say that the > style> of object-oriented programming that C++, Java, or C# users would expect is not present in either Go or Rust.
—Jack Mott


<br>

<font size=1 color="#008080">

pragmatic

adj. 实际的；实用主义的


exclusively

adv. 唯一地，专有地，排外地；作为唯一的（消息）来源


facilities 

n. 设施；工具，设备.此处译为功能比较好


debate 

n. 辩论；（正式的）讨论

v. （尤指正式）讨论，辩论；仔细考虑

</font>

<br>




### Development at scale

<br>

Both Rust and Go have some useful features which make them suitable for programming in the large, whether that means large teams, or large codebases, or both.

For example, whereas C programmers have argued for years about where to put their brackets, and whether code should be indented with tabs or spaces, both Rust and Go eliminate such issues completely by using a standard formatting tool (gofmt for Go, rustfmt for Rust) which rewrites your code automatically using the canonical style. It’s not that this particular style is so wonderful in itself: it’s the standardisation which Rust and Go programmers appreciate.

> gofmt> ’s style is no one’s favourite, yet > gofmt> is everyone’s favourite.  —> Rob Pike

Another area where both languages score highly is in the build pipeline. Both have excellent, built-in, high-performance standard build and dependency management tools; no more wrestling with complex third-party build systems and having to learn a new one every couple of years.

Building Go and Rust code, having come from a Java and Ruby background in my early career, felt like an impossible weight off my shoulders. When I was at Google, it was a relief to come across a service that was written in Go, because I knew it would be easy to build and run. This has also been true of Rust, though I've only worked on that at much smaller scale. I'm hoping that the days of infinitely configurable build systems are dead, and languages all ship with their own purpose-built build tools that just work out of the box.
—> Sam Rose


<br>

<font size=1 color="#008080">

codebase  

n. 代码库；


indented 

adj. 犬牙交错的；受契约约束的；缩进排印的

v. 缩进；切割成锯齿状（indent的过去分词


eliminate  

vt. 消除；排除


canonical 

adj. 按照宗法教规的；真经的；经典的；（数学表达式）最简洁的；准确的；权威的；公认的；与公理有关的；与教会有关的，与教士有关的

n. 教士的法衣（常复数）


standardisation  

n. 标准化


</font>

<br>


>  gofmt> ’s style is no one’s favourite, yet > gofmt> is everyone’s favourite.  —> [Rob Pike](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=8m43s)


Another area where both languages score highly is in the build pipeline. Both have excellent, built-in, high-performance standard build and dependency management tools; no more wrestling with complex third-party build systems and having to learn a new one every couple of years.

> Building Go and Rust code, having come from a Java and Ruby background in my early career, felt like an impossible weight off my shoulders. When I was at Google, it was a relief to come across a service that was written in Go, because I knew it would be easy to build and run. This has also been true of Rust, though I've only worked on that at much smaller scale. I'm hoping that the days of infinitely configurable build systems are dead, and languages all ship with their own purpose-built build tools that just work out of the box.
—> [Sam Rose](https://samwho.dev/)


<font size=1 color="#008080">


built-in

adj. 是…的组成部分的; 嵌入式的; 内置的;


wrestling 

n. 摔跤；扭斗


an impossible weight off my shoulders

不能承受之重


relief  

n. 救济；减轻，解除；安慰；浮雕


ship (with)

此处译为附带比较好


</font>



<br>


## So what’s all the fuss about?

<br>

With all that in mind, and seeing that both languages are so well-designed and powerful, you might be wondering what all the holy wars are about (me too). Why do people make such a fuss about 'Go versus Rust', getting into angry social media spats and writing long blog posts about how only an idiot would use Rust, or that Go isn't a real programming language, or whatever. It might make them feel better, but it doesn't exactly help you, as someone trying to decide which language to use for your project, or which you should learn to advance your programming career. A wise person doesn't make important choices based on who shouts the loudest.

Let's continue our grown-up discussion now by looking at some areas where a reasonable person might prefer one language over the other.


<br>


<font size=1 color="#008080">

With all that in mind

考虑到所有这些


holy wars

圣战


fuss  

n. 大惊小怪；反对，抗议；繁琐的手续；（为小事）大发牢骚

v. 瞎操心；忙乱；打扰；过分关怀；过于讲究细节；吵闹


spats   

n. 护脚；争执（spat的复数）

v. 溅；轻打；争吵（spat的第三人称单数）


advance your programming career

促进你的编程事业


grown-up

adj.	成熟的; 成年的; 长大的; 适于成人的; 成年人特有的;

n.	大人; 成人;

</font>

<br>


## Performance

<br>


We've said that both Go and Rust produce extremely fast programs because they're compiled to native machine code, without having to go through an interpreter or virtual machine. However, Rust's performance is particularly outstanding. It's comparable with C and C++, which are often regarded as the highest-performance compiled languages, but unlike these older languages, it also offers memory safety and concurrency safety at essentially no cost in execution speed. Rust also allows you to create complex abstractions without paying a performance penalty at run-time.

By comparison, although Go programs also perform extremely well, Go is primarily designed for speed of development (including compilation), rather than speed of execution. The Go compiler doesn’t spend a lot of time trying to generate the most efficient possible machine code; it cares more about compiling lots of code quickly. So Rust will usually beat Go in run-time benchmarks.

Rust’s run-time performance is also consistent and predictable, because it doesn’t use garbage collection. Go’s garbage collector is very efficient, and it’s optimised to make its stop-the-world pauses as short as possible (and getting shorter with every new Go release). But garbage collection inevitably introduces some unpredictability in the way programs behave, which can be a serious issue in some applications, such as embedded systems.

Because Rust aims to give the programmer complete control of the underlying hardware, it's possible to optimise Rust programs to be pretty close to the maximum theoretical performance of the machine. This makes Rust an excellent choice for areas where speed of execution beats all other considerations, such as game programming, operating system kernels, web browser components, and real-time control systems.


<br>

<font size=1 color="#008080">

By comparison   相比之下


consistent  [kənˈsɪstənt] 

adj. 始终如一的，一致的；坚持的



predictable  

adj. 可预言的,可预测的


inevitably  

adv. 不可避免地；必然地


unpredictability 

n. 不可预测性，不可预知性；不可预见性


embedded systems 嵌入式系统


</font>


<br>


## Simplicity

<br>


It doesn't matter how fast a programming language is if nobody can figure out how to use it. Go was deliberately conceived as a reaction against the ever-growing complexity of languages like C++; it has very little syntax, very few keywords, and, indeed, few features. This means it doesn't take long to learn the Go language to the point where you can write useful programs in it.



> Go is incredibly easy to learn. I know this is an often-touted benefit, but I was really surprised at how quickly I was able to be productive. Thanks to the language, docs, and tools, I was writing interesting, committable code after literally two days.
—> [Early Impressions of Go From a Rust Programmer](https://medium.com/better-programming/early-impressions-of-go-from-a-rust-programmer-f4fd1074c410)



The key word here is simplicity. *Simple isn't the same as easy*, for sure, but a small, simple language is easier to learn than a large, complex one. There just aren't that many different ways to do things, so all well-written Go code tends to look the same. It’s easy to just dive into an unfamiliar service and understand what it’s doing.


<br>

<font size=1 color="#008080">


figure out

想出; 理解; 弄清;


deliberately  

adv. 故意地；缓慢而谨慎地


reaction 


n. 反应，感应；反动，复古；反作用


incredibly 

adv. 难以置信地；非常地


literally  

adv. 照字面地；逐字地；不夸张地；正确地；简直

</font>


<br>



```go
fmt.Println("Gopher's Diner Breakfast Menu")
for dish, price := range menu {
    fmt.Println(dish, price)
}
```


(In the [Code Club](https://bitfieldconsulting.com/videos) series, I do exactly that: pick Go projects semi-randomly from GitHub and explore them with a group of Go beginners, to see how much of the code we can understand. It always turns out to be more than we expected!)

Although the core language is small, Go’s standard library is very powerful. That means your learning curve will also need to include the parts of the standard library that you need, not just Go syntax. On the other hand, moving functionality out of the language and into the standard library means that you can focus on learning only the libraries that are relevant to you right now.

Go is also designed for software development at scale, with large codebases and large teams. In these situations, it's important that new developers can get up to speed as quickly as possible.

> With Go, you get things done—fast. Go is one of the most productive languages I've ever worked with. The mantra is: > solve real problems today> .
—> [Matthias Endler](https://endler.dev/2017/go-vs-rust/)


<font size=1 color="#008080">

relevant 

adj. 相关的；切题的；中肯的；有重大关系的；有意义的，目的明确的


mantra 

n. 咒语（尤指四吠陀经典内作为咒文或祷告唱念的）；颂歌

此处翻译为 口头禅 较好


</font>


<br>


## Features

<br>


> Rust supports more complexity than several other programming languages, therefore, you can achieve more with it. For example, it supports generics.
—> [Devathon](https://devathon.com/blog/rust-vs-go-which-programming-language-to-choose/)

Rust is specifically designed to include lots of powerful and useful features to help programmers do the most with the least code. For example, Rust's match feature lets you write flexible, expressive logic quite concisely:

<br>

```rust
fn is_prime(n: u64) -> bool {
    match n {
        0...1 => false,
        _ => !(2..n).any(|d| n % d == 0),
    }
}
```


Because Rust does a lot, this means there's a lot to learn, especially at the start. But that's okay: there's a lot to learn in C++ or Java, too, and you don't get the advanced features that come with Rust, like memory safety. To criticise Rust for being a complex language misses the point: it's designed to be expressive, which means having a lot of features, and in many situations that's what you want from a programming language. There's a learning curve, for sure, but once you’re up and running with it, you’ll be fine.

> Rust competes for mindshare with C++ and D for programmers who are prepared to accept more complex syntax and semantics (and presumably higher readability costs) in return for the maximum possible performance.
—> [Dave Cheney](https://dave.cheney.net/2015/07/02/why-go-and-rust-are-not-competitors)

<br>


<font size=1 color="#008080">

expressive 

adj. 表现的；有表现力的


concisely 

adv. 简明地，简洁地


learning curve

学习曲线


presumably  

adv. 大概；推测起来；可假定


</font>


<br>



## Concurrency

<br>


Most languages have some form of support for concurrent programming (doing multiple things at once), but Go was designed for this job from the ground up. Instead of using operating system threads, Go provides a lightweight alternative: goroutines. Each goroutine is an independently executing Go function, which the Go scheduler will map to one of the OS threads under its control. This means that the scheduler can very efficiently manage a large number of concurrent goroutines, using only a limited number of OS threads.

Consequently, you can run millions of concurrent goroutines in a single program without creating serious performance problems. That makes Go the perfect choice for high-scale concurrent applications such as webservers and microservices.

Go also features fast, safe, efficient ways for goroutines to communicate and share data, using channels. Go's concurrency support feels well-designed, and a pleasure to use. Reasoning about concurrent programs is hard in general, and building reliable, correct concurrent programs is a challenge in any language. Because it was built into the language from the start, though, instead of being an afterthought, concurrent programming in Go is about as simple and well-integrated as it could reasonably be.


> Go makes it very easy to build a nicely factored application that takes full advantage of concurrency while being deployed as a set of microservices. Rust can do those things, too, but it’s arguably a bit tougher. In some respects, Rust’s obsession with preventing memory-related security vulnerabilities means that programmers have to go out of their way to perform tasks that would be simpler in other languages, including Go.
—> [Sonya Koptyev](https://sdtimes.com/softwaredev/the-developers-dilemma-choosing-between-go-and-rust/)


The concurrency story in Rust is very new, by comparison, and still stabilising, but it’s under very active development, so watch this space. For example, Rust’s [rayon](https://github.com/rayon-rs/rayon) library provides a very elegant and lightweight way of turning sequential computations into parallel ones.

> Having lightweight syntax for spawning Go routines and using channels are really nice. It really shows the power of syntax that such small details make concurrent programming feel so much nicer than in other languages.
—> [Early Impressions of Go From a Rust Programmer](https://medium.com/better-programming/early-impressions-of-go-from-a-rust-programmer-f4fd1074c410)


While it may be a bit less straightforward to implement concurrent programs in Rust, it’s still possible, and those programs can take advantage of Rust’s guarantees about safety. A good example is the standard library’s Mutex class: in Go, you can forget to obtain a mutex lock before accessing something, but Rust won’t let you do that.

> Go is focused on concurrency as a first class concept. That is not to say you cannot find aspects of Go’s actor oriented concurrency in Rust, but it is left as an exercise to the programmer.
—> [Dave Cheney](https://dave.cheney.net/2015/07/02/why-go-and-rust-are-not-competitors)






<br>

<font size=1 color="#008080">


 [from the ground up](https://www.sohu.com/a/230020303_497891)


afterthought  

n. 事后的想法，马后炮；后来添加的东西


rayon  

n. 人造丝；人造纤维


sequential

adj. 顺序的


parallel

adj.并行的


straightforward 

adj. 简单的；坦率的；明确的；径直的

adv. 直截了当地；坦率地


</font>

<br>


## Safety

<br>


We saw earlier that both Go and Rust aim, in different ways, to prevent a large class of common programming errors, to do with memory management. But Rust in particular goes to great lengths to ensure that you can't do something unsafe that you didn't mean to do.

Rust's very strict and pedantic compiler checks each and every variable you use and every memory address you reference. It avoids possible data race conditions and informs you about undefined behavior. Concurrency and memory safety issues are fundamentally impossible to get in the safe subset of Rust.
—> [Why Rust?](https://bitbucket.org/blog/why-rust)

This is going to make programming in Rust a different experience to almost all other languages, and it may be a challenging one at first. But for many people, the hard work is worth it.

> For me the key advantage of Rust is a feeling that the compiler has my back and won't let through any bug it could possibly detect (seriously, it feels like magic sometimes).
—Grzegorz Nosek

Many languages, including Go, have facilities to help programmers avoid mistakes, but Rust takes this to a new level, so that potentially incorrect programs won’t even compile.

> With Rust, the library programmer has a lot of tools to prevent her users making mistakes. Rust gives us the ability to say that we > own> a specific piece of data; it's not possible for anything else to claim ownership, so we know nothing else will be able to modify it. I can't think of a time I've ever been given this many tools to prevent accidental misuse before. It's a wonderful feeling.
—> [Sam Rose](https://samwho.dev/)

"Fighting with the borrow checker" is a common syndrome for new Rust programmers, but in most cases the problems that it finds are genuine bugs (or at least potential bugs) in your code. It may force you to fundamentally re-architect your program to avoid running into these issues; and that's a good thing, when correctness and reliability are your top priority. What's the point of a language that doesn't change the way you program? The lessons that Rust teaches about safety can be useful when you’re working in other languages, too.

If you choose Rust, usually you need the guarantees that the language provides: safety against null pointers and data races, predictable runtime behaviour, and total control over the hardware. If you don't require any of these features, Rust might be a poor choice for your next project. That's because these guarantees come with a cost: ramp-up time. You'll need to unlearn bad habits and learn new concepts. Chances are, you will fight with the borrow checker a lot when you start out.
—> [Matthias Endler](https://endler.dev/2017/go-vs-rust/)

How challenging you find Rust's programming model probably depends on what previous experience you have in other languages. Python or Ruby programmers may find it restrictive; others will be delighted.

> If you’re a C or C++ programmer who’s spent weeks chasing down memory safety bugs in those languages, you’ll really appreciate Rust. “Fighting the borrow checker” becomes “The compiler can detect that? Cool!”
—Grzegorz Nosek

<br>


<font size=1 color="#008080">


prevent  

vt. 预防，防止；阻止
vi. 妨碍，阻止


pedantic  

adj. 迂腐的；学究式的；卖弄学问的；假装学者的



subset  

n. [数] 子集；子设备；小团体



syndrome  

n. [临床] 综合症状；并发症状；校验子


restrictive  

adj. 限制的；限制性的；约束的

n. 限制词


</font>


<br>

## Scale

<br>


> Today's server programs comprise tens of millions of lines of code, are worked on by hundreds or even thousands of programmers, and are updated literally every day. Go was designed and developed to make working in this environment more productive. Go's design considerations include rigorous dependency management, the adaptability of software architecture as systems grow, and robustness across the boundaries between components.
—> [Rob Pike](https://talks.golang.org/2012/splash.article)

When you're working on a problem by yourself or in small teams, the choice of a simple language or a rich language is a matter of preference. But as the software grows bigger and more complex, and the teams grow larger, the differences really start to show. For large applications and distributed systems, speed of execution is less important than speed of development: a deliberately minimal language like Go reduces the ramp-up time for new developers, and makes it easier for them to work with a large codebase.

> With Go, it’s easier as a junior developer to be more productive, and harder as a mid-level developer to introduce brittle abstractions that will cause problems down the line. For these reasons, Rust is less compelling than Go for enterprise software development.
—> [Loris Cro](https://kristoff.it/blog/why-go-and-not-rust/)

When it comes to software development in the large, clear is better than clever. Go's limitations actually make it more suitable for enterprises and big organisations than more complex and powerful languages such as Rust.



<br>

<font size=1 color="#008080">

comprise 

vt. 包含；由…组成


tens of millions of 数以千万计的


literally  

adv. 照字面地；逐字地；不夸张地；正确地；


rigorous 

adj. 严格的，严厉的；严密的；严酷的


a deliberately minimal language

故意最小化的语言


brittle 

adj. 易碎的，脆弱的；易生气的


[down the line](https://baijiahao.baidu.com/s?id=1667569298863707890&wfr=spider&for=pc)

[词典]	其间某时; 在某一时刻; 在某一环节;<br>
[例句]We'll make a decision on that further down the line.
我们将在以后的阶段对此问题作出决策。


compelling 

adj. 引人注目的；令人信服的；非常强烈的；不可抗拒的


</font>


<br>


## The differences

<br>


Although Rust and Go are both popular, modern, widely-used languages, they're not really competitors, in the sense that they're deliberately targeting quite different use cases. Go's whole approach to programming is radically different to Rust's, and each language will suit some people while irritating others. That's absolutely fine, and if both Rust and Go did more or less the same things in more or less the same way, we wouldn't really need two different languages.

So can we get a sense of the respective natures of Rust and Go by finding issues on which they take drastically different approaches? Let's find out.


<br>


<font size=1 color="#008080">

approach  

v. 走进；与……接洽；处理；临近，逐渐接近（某时间或事件）；几乎达到（某水平或状态）

n. 方法，方式；接近；接洽；（某事的）临近；路径；进场（着陆）；相似的事物

此处译为 方法,方式


radically  

adv. 根本上；彻底地；以激进的方式


irritating 

adj. 刺激的；气人的；使愤怒的；烦人的，使人恼火的

vt. 刺激；激怒；使烦恼；使发炎，使不适；使不耐烦；使（身体某部分）疼痛（irritate 的现在分词）


respective  

adj. 分别的，各自的



drastically 

adv. 彻底地；激烈地



</font>


<br>

### Garbage collection

<br>


“To garbage-collect, or not to garbage-collect” is one of those questions that has no right answer. Garbage collection, and automatic memory management in general, makes it quick and easy to develop reliable, efficient programs, and for some people that makes it essential. But others say that garbage collection, with its performance overhead and stop-the-world pauses, makes programs behave unpredictably at run-time, and introduces unacceptable latency. The debate rumbles on.

> Go is a very different language to Rust. Although both can vaguely be described as systems languages or replacements for C, they have different goals and applications, styles of language design, and priorities. Garbage collection is a really huge differentiator. Having GC in Go makes the language much simpler and smaller, and easy to reason about.<br>
Not having GC in Rust makes it really fast (especially if you need guaranteed latency, not just high throughput) and enables features and programming patterns that are not possible in Go (or at least not without sacrificing performance).
—> [PingCAP](https://medium.com/better-programming/early-impressions-of-go-from-a-rust-programmer-f4fd1074c410)



<br>


<font size=1 color="#008080">

The debate rumbles on.

争论仍在继续/争论不休。



vaguely 

adv. 含糊地；暧昧地；茫然地


throughput  

n. （某一时期内的）生产量，接待人数，吞吐量


</font>




<br>

### Close to the metal

<br>


The history of computer programming has been a story of increasingly sophisticated abstractions that let the programmer solve problems without worrying too much about how the underlying machine actually works. That makes programs easier to write and perhaps more portable. But for many programs, access to the hardware, and precise control of how the program is executed, are more important. Rust aims to let programmers get “closer to the metal”, with more control, but Go abstracts away the architectural details to let programmers get closer to the problem.

Both languages have a different scope. Golang shines for writing microservices and for typical "DevOps" tasks, but it is not a systems programming language. Rust is stronger for tasks where concurrency, safety and/or performance are important; but it has a steeper learning curve than Go.
—> [Matthias Endler](https://endler.dev/2017/go-vs-rust/)

<br>

<font size=1 color="#008080">

sophisticated 

adj. 复杂的；精致的；久经世故的；富有经验的
v. 使变得世故；使迷惑；篡改（sophisticate的过去分词形式）



underlying 
adj. 潜在的；根本的；在下面的；优先的

v. 放在…的下面；为…的基础；优先于（underlie的ing形式）

在此译为'底层的'


typical  

adj. 典型的；特有的；象征性的


steeper 

n. 浸泡用的容器

adj. 更陡峭的，更险峻的；更急剧的；更不合理的


</font>




<br>


### Must go faster

<br>


I’ve written elsewhere that performance is less important than readability for most programs. But when performance does matter, it really matters. Rust makes a number of design trade-offs to achieve the best possible execution speed. By contrast, Go is more concerned about simplicity, and it’s willing to sacrifice some (run-time) performance for it. But Go’s build speed is unbeatable, and that’s important for large codebases.

Rust is faster than Go. In the benchmarks above, Rust was faster, and in some cases, an order of magnitude faster. But before you run off choosing to write everything in Rust, consider that Go wasn’t that far behind it in many of those benchmarks, and it’s still much faster than the likes of Java, C#, JavaScript, Python and so on.

If what you need is top-of-the-line performance, then you’ll be ahead of the game choosing either of these two languages. If you’re building a web service that handles high load, that you want to be able to scale both vertically and horizontally, either language will suit you perfectly.
—> [Andrew Lader](https://codeburst.io/should-i-rust-or-should-i-go-59a298e00ea9)


<br>


<font size=1 color="#008080">

trade-offs

折中


an order of magnitude 

(快)一个数量级



be able to scale both vertically and horizontally

能够纵向和横向扩展


</font>




<br>


### Correctness

<br>


On the other hand, a program can be arbitrarily fast if it doesn’t have to work properly. Most code is not written for the long term, but it’s often surprising how long some programs can stay running in production: in some cases, many decades. In these situations it’s worth taking a little extra time in development to make sure that the program is correct, reliable, and won’t need a lot of maintenance in the future.

> My take: Go for the code that has to ship tomorrow, Rust for the code that has to keep running untouched for the next five years.
—Grzegorz Nosek

While both Go and Rust are great choices for any serious project, it's a good idea to make yourself as well-informed as possible about each language and its characteristics. Ultimately, it doesn't matter what anyone else thinks: only you can decide which is right for you and your team.

If you want to develop faster, perhaps because you have many different services to write, or you have a large team of developers, then Go is your language of choice. Go gives you concurrency as a first-class citizen, and does not tolerate unsafe memory access (neither does Rust), but without forcing you to manage every last detail. Go is fast and powerful, but it avoids bogging the developer down, focusing instead on simplicity and uniformity. If on the other hand, wringing out every last ounce of performance is a necessity, then Rust should be your choice.
—> [Andrew Lader](https://codeburst.io/should-i-rust-or-should-i-go-59a298e00ea9)


<br>


<font size=1 color="#008080">

arbitrarily 

adv. 武断地；反复无常地；专横地



well-informed


adj.	博识的；确知的；消息灵通的;

[例句]This is a subject for serious, well-informed discussion, not tabloid headlines.

这个主题不同于通俗小报的标题，适合进行严肃而深入的讨论。



characteristics 

n. 特性，特征；特色（characteristic的复数）；特质



bogging   

n. 沼泽土化；陷入；沉入

v. 陷入泥沼；陷入困境（bog的ing形式）



uniformity 

n. 均匀性；一致；同样

在此译为'统一性'较好



</font>

<br>


## Conclusion

<br>


I hope this article has convinced you that both Rust and Go deserve your serious consideration. If at all possible, you should aim to get at least some level of experience in both languages, because they will be incredibly useful to you in any tech career, or even if you enjoy programming as a hobby. If you only have time to invest in learning one language well, don't make your final decision until you've used both Go and Rust for a variety of different kinds of programs, large and small.

And knowledge of a programming language is really only a small part of being a successful software engineer. By far the most important skills you'll need are design, engineering, architecture, communication, and collaboration. If you excel at these, you'll be a great software engineer regardless of your choice of language. Happy learning!

<br>

## Comparing Rust and Go code

<br>

There's a great website called [programming-idioms.org](https://programming-idioms.org/) which has a "cheat sheet" showing what the Rust and Go code looks like for over 200 common programming tasks:

<br>



 [Go vs Rust idioms](https://programming-idioms.org/cheatsheet/Go/Rust)

<br>

## Getting started

<br>


If you're interested in learning to program with Rust and Go, here are a few resources you may find helpful.


<br>

### Go

<br>

- [Install Go](https://golang.org/dl/)
- [Go tutorials by Bitfield](https://bitfieldconsulting.com/golang)
- [For the Love of Go](https://bitfieldconsulting.com/books)
- [A Tour of Go](https://tour.golang.org/)
- [Go By Example](https://gobyexample.com/)
- [The Go Playground](https://play.golang.org/)
- [Awesome Go](https://github.com/avelino/awesome-go)

<br>

### Rust

<br>

- [Install Rust](https://www.rust-lang.org/tools/install)
- [A Gentle Introduction to Rust](https://stevedonovan.github.io/rust-gentle-intro/)
- [The Rust Programming Language](https://doc.rust-lang.org/book/index.html)
- [Rust books](https://github.com/sger/RustBooks)
- [Rust By Example](https://doc.rust-lang.org/rust-by-example/)
- [The Rust Playground](https://play.rust-lang.org/)
- [One Hundred Rust Binaries](https://www.wezm.net/v2/posts/2020/100-rust-binaries/)


<br>


## Acknowledgements

<br>

I am not young enough to know everything, as the saying goes, so I'm very grateful to a number of distinguished Gophers and Rustaceans who took the time to review and correct this piece, as well as providing some really useful suggestions. My special thanks go to Bill Kennedy, Grzegorz Nosek, Sam Rose, Jack Mott, Steve Klabnik, MN Mark, Ola Nordstrom, Levi Lovelock, Emile Pels, Sebastian Lauwers, Carl Lerche, and everyone else who contributed. You might get the impression from reading online hot takes that the Rust and Go communities don't get along. Nothing could be further from the truth, in my experience; we had very civilised and fruitful discussions about the draft article, and it's made a big difference to the finished product. Thanks again.


John Arundel


<br>


<font size=1 color="#008080">

civilised  

adj. 文明的


fruitful 

adj. 富有成效的；多产的；果实结得多的


finished product 成品,最终作品


</font>



<br>



