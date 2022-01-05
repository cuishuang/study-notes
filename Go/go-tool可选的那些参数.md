---
title: go tool可选的那些参数
date: 2018-05-04 21:23:25
tags: Go
categories: runtime
---





<br>





go tool asm file 将go汇编文件编译为 object（.o） 文件。

go tool compile file 将go文件编译为 .o 文件。

go tool compile -N -l -S file 将文件编译为汇编代码

或者使用：go build -gcflags -S x.go

gcflags == go compile flags

go tool compile：处理go文件，执行词法分析、语法分析、汇编、编译，输出obj文件

go tool asm：处理汇编文件（.s文件），输出obj文件

go tool pack：打包package下的所有obj文件，输出.a文件

go tool link：链接不同package的.a文件，输出可执行文件

go tool objdump：反汇编obj文件

go tool nm：输出obj文件、.a文件或可执行文件中定义的符号





˙

[Go 语言工具](https://iswade.github.io/go/go_tools/)

官方文档 [Command compile](https://golang.org/cmd/compile/)

[得到Go程序的汇编代码的方法](https://colobu.com/2018/12/29/get-assembly-output-for-go-programs/)