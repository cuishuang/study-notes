---
title: golang之struct入门
date: 2017-11-26 18:52:02
tags: [Go,Pearl]
---
<br>

### 起步

[基础起步点此](https://blog.csdn.net/weixin_34387284/article/details/91261098)

---

#### 结构体判等
<br>

只有在结构体的所有字段类型全部支持**直接判等**时，才可做判断操作。

map，slice不支持直接判等，需借助reflect.DeepEqual来比较(map整个是一个指针(*hmap), slice是SliceHeader的Data字段是个指针)


```go
package main

import "fmt"

func main() {
	type data struct {
		x int
	}

	d1 := data{
		x: 100,
	}

	d2 := data{
		x: 100,
	}
	fmt.Println(d1 == d2)
}
```
执行结果为:`true`
<br>


```go
package main

import "fmt"

func main() {
	type data struct {
		x int
		y map[string]int //字典类型不支持==,
	}

	d1 := data{
		x: 100,
	}

	d2 := data{
		x: 100,
	}
	fmt.Println(d1 == d2) //struct containing map[string]int cannot be compared
}
```
编译不通过:` invalid operation: d1 == d2 (struct containing map[string]int cannot be compared)
`

---


#### 空结构体
<br>

空结构struct{}是指没有字段的结构类型。<br>
可作为通道元素类型，用于事件通知。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	exit := make(chan struct{})

	go func() {
		for {
			fmt.Println("hello, world!")
			exit <- struct{}{}

		}
	}()

	<-exit

	time.Sleep(2e9)
	fmt.Println("end.")
}
```
输出为:
```
hello, world!
hello, world!
end.
```

<br>

---


#### 指针操作

<br>
可使用指针直接操作(取值/赋值等)结构体字段，但不能是多级指针。

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	type user struct {
		name string
		age  int
	}

	p := &user{ //获取指针
		name: "fliter",
		age:  26,
	}

	fmt.Println(p) //&{fliter 26}

	p.name = "jack" //通过指针找到对应的程序实体
	p.age--
	fmt.Println(p) //&{jack 25}

	fmt.Println(*p) //{jack 25}

	p2 := &p                        //&p属于二级指针
	fmt.Println(p2)                 //0xc00000e020
	fmt.Println(reflect.TypeOf(p2)) //**main.user
	//*p2.name = "john" //p2.name undefined (type **user has no field or method name)
}
```

输出为:
```
&{fliter 26}
&{jack 25}
{jack 25}
0xc00007c010
**main.user

```


---



### Struct中的tag


<br>

可以为struct中的每个字段，加一个tag。这个tag可以通过反射机制获取到，最常用场景(包括但绝不仅限于此,如**还常用于格式校验,数据库关系映射**等)就是 <font color="red">json序列化和反序列化</font>.

```go
package main

import (
	"fmt"
	"reflect"
)

type User struct {
	name string `姓名`
	sex  string `性别`
	age  int    `年龄`
}

func main() {
	u := User{"Messi", "男", 32}
	v := reflect.ValueOf(u)
	t := v.Type()

	for i, n := 0, t.NumField(); i < n; i++ {
		fmt.Printf("%s为: %v\n", t.Field(i).Tag, v.Field(i))
	}
}
```

输出为:

```
姓名为: Messi
性别为: 男
年龄为: 32
```
---
<br>

#### 用于json序列化
<br>
**注意:**

<font color="red" face="STHeiti">struct转json时,struct里的字段名首字母必须大写,否则无法正常解析;如果想让struct转json后字段名首字母小写,可以通过tag指定</font>

<br>
[更多可点击](https://studygolang.com/articles/13455)

```go
package main

import (
	"encoding/json"
	"fmt"
)

type FiveAlevelArea struct {
	Name        string  `json:"name"`
	Location    string  `json:"address"`
	Price       float32 `json:"entrance ticket"`
	englishName string  `json:"english_name"`
}

func main() {

	heBei := FiveAlevelArea{
		"承德市双桥区承德避暑山庄及周围寺庙景区",
		"承德市山庄南路",
		100.00,
		"Chengde Imperial Summer Resort",
	}

	j, err := json.Marshal(heBei)

	if err != nil {
		fmt.Println("报错如下:", err)
		return
	}

	fmt.Println("json字符串为:", string(j))

}
```

输出为:
```
json字符串为: {"name":"承德市双桥区承德避暑山庄及周围寺庙景区","address":"承德市山庄南路","entrance ticket":100}
```

***可见首字母不为大写的字段名,没有被解析出来;最后json字符串中展示出的键名,是tag标签里指定的名称***

---


### 匿名字段

<br>

所谓匿名字段是指没有名字，仅有类型的字段，也称作`嵌入字段`或`嵌入类型`。
从编译器角度看，这只是隐式地以类型名作为字段名称。
可直接引用匿名字段的成员，但初始化时必须当作独立字段。

匿名字段不仅仅可以是结构体，除"接口指针"和"多级指针"以外的任何命名类型都可以作为匿名字段。


严格说来，Go并不是传统意义上的面向对象编程语言，或者说仅实现了最小面向对象的机制。
匿名嵌入不是继承，无法实现多态处理。
虽然配合方法集，可用接口来显现一些类似的操作，但其本质完全不同。

```go
package main

import "fmt"

type attr struct {
	perm int
}

type file struct {
	name string
	attr //匿名字段,仅有类型名(但IDE在格式化此字段时,不会和其他字段的类型对齐,而会和其他字段的名称对齐)
}

func main() {

	//方式1:
	var c file
	c.name = "my.txt"
	var a attr
	a.perm = 777
	c.attr = a
	fmt.Println(c, c.perm) //直接读取匿名字段成员

	//方式2:
	//如果使用这种方式给包含匿名字段的结构体赋值,须将类型名当作字段名
	f := file{
		name: "test.dat",
		attr: attr{ //将类型名当作字段名
			perm: 755,
		},
	}
	f.perm = 500           //直接设置匿名字段成员
	fmt.Println(f, f.perm) //直接读取匿名字段成员

}
```



---

#### 利用**匿名字段**实现所谓的"继承"

<br>

结构体中字段可以没有名字(只有类型)，即匿名字段;

如果一个struct(记为A)嵌套了另一个匿名结构体(记为B)，那么A结构体可以直接访问 
匿名结构体B的方法，从而实现了继承。

[参见前文,golang利用组合实现继承,和php或java面向对象的继承有何不同](http://www.dashen.tech/2017/07/08/golang%E5%88%A9%E7%94%A8%E7%BB%84%E5%90%88%E5%AE%9E%E7%8E%B0%E7%BB%A7%E6%89%BF-%E5%92%8Cphp%E6%88%96java%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%BB%A7%E6%89%BF%E6%9C%89%E4%BD%95%E4%B8%8D%E5%90%8C/)


---

### 方法

<br>
Golang中的方法作用在特定类型的变量上，因此自定义类型都可以 
有方法，而不仅仅是struct

定义：`func (recevier type) methodName(参数列表)(返回值列表){}`

<br>


#### `方法`和`函数`

<br>

golang中的"方法"与"函数"的关系,与其他语言有所差别.


方法 给用户定义的类型添加新的行为。方法和函数相比，声明时在关键字func和方法名之间增加了一个参数

```go
package main

import "fmt"

type user struct {
	name string
	age  int
}

func (u user) say() {
	fmt.Println("hello", u.name)
	fmt.Println("十年后你的年龄是:", u.age+10)
}

func (u *user) run() {
	fmt.Printf("%s是跑步名将\n", u.name)

}

func main() {
	c := user{"Trump", 73}
	c.say()

	s := &user{"Bolt", 0}

	s.run()

}
```

输出:

```
hello Trump
十年后你的年龄是: 83
Bolt是跑步名将
```

- 一个基础类型前面加&后,会变为指针类型;<br>
- *作动词时, 后面必须是一个指针类型,叫做解引用~<br>

> 即 *(&a) = a


- *作形容词时,表示这个类型是指针类型
---

#### 方法作用在结构体上
<br>

```go
package main

import (
	"fmt"
)

type FiveAlevelArea struct {
	Name        string  `json:"name"`
	Location    string  `json:"address"`
	Price       float32 `json:"entrance ticket"`
	englishName string  `json:"english_name"`
}

func (cs FiveAlevelArea) set(Name, Location, englishName string, Price float32) {

	cs.Name = Name
	cs.Location = Location
	cs.Price = Price
	cs.englishName = englishName

	fmt.Println("当前结构体的值为:", cs)
}

func (shuang FiveAlevelArea) get() FiveAlevelArea {
	return shuang
}

func main() {
	var heBei FiveAlevelArea
	heBei.set("保定市安新县白洋淀景区", "保定市安新县", "Bai-yang Lake", 40.00)

	heBei2 := heBei.get()

	fmt.Println(heBei)
	fmt.Println(heBei2)
}
```

输出为:

```
当前结构体的值为: {保定市安新县白洋淀景区 保定市安新县 40 Bai-yang Lake}
{  0 }
{  0 }
```

**<font color="red" face="STHeiti">因为是值传递,故而原来变量的值没有被修改;</font>**

改为指针传递:将第14行set()方法作用的变量(类型)修改为`cs *FiveAlevelArea`,此时程序的执行结果为:

```
当前结构体的值为: &{保定市安新县白洋淀景区 保定市安新县 40 Bai-yang Lake}
{保定市安新县白洋淀景区 保定市安新县 40 Bai-yang Lake}
{保定市安新县白洋淀景区 保定市安新县 40 Bai-yang Lake}
```

可能会疑惑,第32行调用set()方法的变量类型为`FiveAlevelArea`,并不是指针,为何还能编译通过呢?实际上是go自动加了取指针符号&,即第32行:

`heBei.init("保定市安新县白洋淀景区", "保定市安新县", "Bai-yang Lake", 40.00)`

等同于:

`(&heBei).init("保定市安新县白洋淀景区", "保定市安新县", "Bai-yang Lake", 40.00)`


---

#### 方法作用在其他变量上
<br>


```go
package main

import "fmt"

type cInt int

func (cui cInt) f1() {
	fmt.Println("变量值为:", cui)
}

func (c *cInt) f2(args cInt) {
	*c = args + 6
}

func main() {

	var a1 cInt = 10086

	fmt.Println(a1)

	//调用cInt类型的f1()方法
	a1.f1()



	var a2 cInt

	//&a2是获取变量a2的内存地址值即指针，如果对一个参数需要接收指针(*后面必须要是指针类型)的方法如此处的f2()传值a2,go会自动将a2转换成&a2即变量a2的内存地址,所以以下两种写法都是可以的
	a2.f2(12300)
	fmt.Println(a2)

	(&a2).f2(12300)
	fmt.Println(a2)


	//指针传递可以改变原来变量的值
	var a3 cInt = 12360
	a3.f2(a3)
	fmt.Println(a3)
	(&a3).f2(a3)
	fmt.Println(a3)

}
```

输出为:

```
10086
变量值为: 10086
12306
12306
12366
12372

```

---

#### String()方法

<br>

***<font color="red" face="STHeiti">如果一个变量实现了String()这个方法，那么fmt.Println在输出时，会默认调用这个变量的String()方法。</font>***

```golang
package main

import "fmt"

type Transportation struct {
	name  string
	speed string
}

func (o *Transportation) Run() {

	fmt.Printf("交通工具%s跑起来啦!\n", (*o).name) //此处"o.name"同样可以通过编译(o是一个指针,*o解引用为这个内存地址对应的变量),go会自动转化为(*o).name
}

//火车继承交通工具
type Train struct {
	Transportation
	InventedTime int //发明时间
}

func (cs *Train) String() string {
	str := fmt.Sprintf("%s的速度可以达到%s\n", cs.name, cs.speed)
	return str
}

func main() {
	var train Train

	train.name = "火车"
	train.speed = "350km/h"
	train.InventedTime = 1804

	fmt.Println(train)

	fmt.Println("------------------")
	train.Run()

	fmt.Println("******************")
	fmt.Printf("%s", &train)

	fmt.Println("##################")
	fmt.Println("会自动调用String()方法\n", &train)

}

```

输出为:

```
{{火车 350km/h} 1804}
------------------
交通工具火车跑起来啦!
******************
火车的速度可以达到350km/h
##################
会自动调用String()方法
 火车的速度可以达到350km/h
```

--- 


[更多关于struct使用中的小tips,可点击](https://note.youdao.com/web/#/file/WEB078b1c055f81873fdd5dbb799a86f101/note/WEB0d5d830a887ab27384bcf4d5121c314f/)
