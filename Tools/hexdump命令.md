---
title: hexdump命令
date: 2017-02-01 18:11:08
tags: Tools
---

比较特殊的**文件内容查看**工具


<font size=1>


hex	英[heks]
美[heks]

v.	施魔法以加害(某人);

n.	十六进制，巫婆，妖法;

[例句]It can display data in ASCII, decimal, hex, and octal formats.

它能够显示ASCII的数据，小数，十六进制和八进制格式。

</font>

<br>

**hexdump**是UNIX下的一个二进制文件查看工具，它可以将二进制文件转换为ASCII、八进制、十进制、十六进制格式进行查看。


一般用来查看“二进制”文件的十六进制编码，但实际上它能查看任何文件，而不只限于二进制文件，也不止限于以十六进制格式查看。



<br>


在分析mysql binlog或者ibd文件时候，常会用到**hexdump** 查看物理文件的存储内容。


<br>


### <font color="#CD853F">语法:</font>

<br>

> hexdump [选项] [文件]...

<br>

---

<br>


### <font color="#CD853F">选项:</font>

<br>


```sql
-n length 只格式化输入文件的前length个字节。
-C 输出规范的十六进制和ASCII码。
-b 单字节八进制显示。
-c 单字节字符显示。
-d 双字节十进制显示。
-o 双字节八进制显示。
-x 双字节十六进制显示。
-s 从偏移量开始输出。
-e 指定格式字符串，格式字符串包含在一对单引号中，格式字符串形如：'a/b "format1" "format2"'。
```

<br>


对于 -e:

每个格式字符串由三部分组成，每个由空格分隔，第一个形如a/b，b表示对每b个输入字节应用format1格式，a表示对每a个输入字节应用format2格式，一般a>b，且b只能为1，2，4，另外a可以省略，省略则a=1。format1和format2中可以使用类似printf的格式字符串，如：

- %02d：两位十进制
- %03x：三位十六进制
- %02o：两位八进制
- %c：单个字符等


还有一些特殊的用法：

- %_ad：标记下一个输出字节的序号，用十进制表示。
- %_ax：标记下一个输出字节的序号，用十六进制表示。
- %_ao：标记下一个输出字节的序号，用八进制表示。
- %_p：对不能以常规字符显示的用 . 代替。

同一行如果要显示多个格式字符串，则可以跟多个-e选项。

<br>

---

<br>


### <font color="#CD853F">实例:</font>

<br>



`hexdump -e '16/1 "%02X " "  |  "' -e '16/1 "%_p" "\n"' test`


```sql
00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F  |  ................  
10 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F  |  ................  
20 21 22 23 24 25 26 27 28 29 2A 2B 2C 2D 2E 2F  |   !"#$%&'()*+,-./ 
```


---


摘自: [hexdump命令](https://man.linuxde.net/hexdump)

参考:

[Linux命令学习总结：hexdump](https://www.cnblogs.com/kerrycode/p/5077687.html)




