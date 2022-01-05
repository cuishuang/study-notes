---
title: 'go-callvis:极其好用的Go程序调用图生成工具'
date: 2016-04-15 20:02:11
tags: Go
---

<br>

接触一个新项目, 阅读官方库或第三方package,经常使用自带的`go list`命令或用[depth](https://github.com/KyleBanks/depth)包来查看其依赖了哪写包,如:

### go list

<img src="go-callvis-极其好用的Go程序调用图生成工具/1.png" width = 80% height = 50% />

<br>

该命令后面的参数`./`下,必须包含`.go`文件. `./`将列出当前目录下所有.go文件的依赖,也可以指定具体某个`.go`文件,如<br>
`go list -json xxx.go`


该参数既可以是指定的本地路径,也可以是本地`go/src`路径下安装的某个第三方package,也可以是官方库,如:

<img src="go-callvis-极其好用的Go程序调用图生成工具/2.png" width = 80% height = 50% />

<br>

<img src="go-callvis-极其好用的Go程序调用图生成工具/3.png" width = 80% height = 50% />


该命令无法列出github公开可访问,但本地没有的package的依赖,如:

<img src="go-callvis-极其好用的Go程序调用图生成工具/4.png" width = 80% height = 50% />

<br>

返回值部分字段的解释:

<img src="go-callvis-极其好用的Go程序调用图生成工具/5.png" width = 80% height = 50% />

<br>

<img src="go-callvis-极其好用的Go程序调用图生成工具/6.png" width = 80% height = 50% />

`Imports`	字符串切片（[]string）:	该代码包中的源码文件显式导入的依赖包的导入路径的列表。

`Deps`	字符串切片（[]string）:	所有的依赖包（包括间接依赖）的导入路径的列表。

只查看某个字段,可以加`-f 双花括号 .字段名 双花括号`命令 (需去掉-json参数),如:

<img src="go-callvis-极其好用的Go程序调用图生成工具/7.png" width = 80% height = 50% />

[更多参考](https://wiki.jikexueyuan.com/project/go-command-tutorial/0.8.html)

---

### depth

<br>
[文档及安装](https://github.com/KyleBanks/depth)

<img src="go-callvis-极其好用的Go程序调用图生成工具/8.png" width = 80% height = 50% />


<br>

<img src="go-callvis-极其好用的Go程序调用图生成工具/9.png" width = 80% height = 50% />

<br>


同样也可以查看go官方库的依赖:
<img src="go-callvis-极其好用的Go程序调用图生成工具/10.png" width = 80% height = 50% />


查看本地go/src路径下已有的包:

<img src="go-callvis-极其好用的Go程序调用图生成工具/11.png" width = 80% height = 50% />

可配合其`-internal`,`-max`,`-explain target-package`,`-json`等可选参数使用.



---

<br>


### go-callvis

以上两个工具,都只能在命令行输出项目的依赖关系,希望能有更直观地图像方式. [go-callvis](https://github.com/TrueFurby/go-callvis)这个包,完美解决了该需求~




<img src="go-callvis-极其好用的Go程序调用图生成工具/12.png" width = 80% height = 50% />


调用关系如下:

<img src="go-callvis-极其好用的Go程序调用图生成工具/13.png" width = 80% height = 50% />

- 绿色是官方package
- 黄色是第三方package



<br>

美中不足是该工具要求指定目录下必须有`main.go`文件,

<img src="go-callvis-极其好用的Go程序调用图生成工具/14.png" width = 80% height = 50% />


默认输出格式为svg,可以通过`-format=png`指定为需要的格式

<img src="go-callvis-极其好用的Go程序调用图生成工具/15.png" width = 80% height = 50% />

<img src="go-callvis-极其好用的Go程序调用图生成工具/16.png" width = 80% height = 50% />