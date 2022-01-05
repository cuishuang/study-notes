---
title: Golang中的defer
date: 2019-08-24 21:14:00
tags: [Go,Pearl]
---

<br>

### 面试常问之defer()的执行次序


<br>

#### 情形1

<br>

```go
package main

func main() {

    defer print(123)
	defer_call()
	defer print(789) //panic之后的代码不会被执行
	print("不会执行到这里")
}

func defer_call() {
	defer func() {
		print("打印前")
	}()
	defer func() {
		print("打印中")
	}()

	defer print("打印后")

	panic("触发异常")

    defer print(666)   //IDE会有提示: Unreachable code
    
}
```

结果为:

```go
打印后打印中打印前123panic: 触发异常

goroutine 1 [running]:
main.defer_call()
	/Users/shuangcui/explore/panicandrecover.go:19 +0xe5
main.main()
	/Users/shuangcui/explore/panicandrecover.go:6 +0x51
```

可见:

* panic之后的defer()不会被执行
* panic之前的defer(),按照**先进后出**的次序执行,最后输出panic信息

(defer机制底层,是用链表实现的一个栈)


再如:

```go
func main() {

	fmt.Println(123)

	defer fmt.Println(999)

	subfunc()

}

func subfunc() {

	defer fmt.Println(888)

	for i := 0; i > 10; i++ {
		fmt.Println("当前i为:", i)
		panic("have a bug")
	}

	defer fmt.Println(456)

}
```

结果为:

```go
123
456
888
999
```

<br>

**defer会延迟到当前函数执行 return 命令前被执行, 多个defer之间按LIFO先进后出顺序执行**

<br>

---

<br>

#### 情形2 (在defer内打印defer之外的主方法里操作的变量)

<br>

```go
package main

import "fmt"

func main() {
	foo()
}

func foo() {
	i := 0
	defer func() {
		//i--
		fmt.Println("第一个defer", i)
	}()

	i++
	fmt.Println("+1后的i：", i)

	defer func() {
		//i--
		fmt.Println("第二个defer", i)
	}()

	i++
	fmt.Println("再+1后的i：", i)

	defer func() {
		//i--
		fmt.Println("第三个defer", i)
	}()

	i++
	fmt.Println("再再+1后的i：", i)

	i = i + 666

	fmt.Println("+666后的i为:", i)

}
```

输出为:

```go
+1后的i： 1
再+1后的i： 2
再再+1后的i： 3
+666后的i为: 669
第三个defer 669
第二个defer 669
第一个defer 669
```

<br>

---



#### 情形3  (在defer内外操作同一变量)

<br>

```go
package main

import "fmt"

func main() {
	foo()
}

func foo() {
	i := 0
	defer func() {
		i--
		fmt.Println("第一个defer", i)
	}()

	i++
	fmt.Println("+1后的i：", i)

	defer func() {
		i--
		fmt.Println("第二个defer", i)
	}()

	i++
	fmt.Println("再+1后的i：", i)

	defer func() {
		i--
		fmt.Println("第三个defer", i)
	}()

	i++
	fmt.Println("再再+1后的i：", i)

	i = i + 666

	fmt.Println("+666后的i为:", i)

}
```


输出为:

```go
+1后的i： 1
再+1后的i： 2
再再+1后的i： 3
+666后的i为: 669
第三个defer 668
第二个defer 667
第一个defer 666
```



<br>



#### 情形4! (发生了参数传递!`---`传递参数给defer后面的函数, defer内外同时操作该参数)

<br>

```go
package main

import "fmt"

func main() {
	foo2()
}

func foo2() {
	i := 0
	defer func(k int) {
		//k--
		fmt.Println("第一个defer", k)
	}(i)

	i++
	fmt.Println("+1后的i：", i)

	defer func(k int) {
		//k--
		fmt.Println("第二个defer", k)
	}(i)

	i++
	fmt.Println("再+1后的i：", i)

	defer func(k int) {
		//k--
		fmt.Println("第三个defer", k)
	}(i)

	i++
	fmt.Println("再再+1后的i：", i)

	i = i + 666

	fmt.Println("+666后的i为:", i)

}
```

输出为:

```go
+1后的i： 1
再+1后的i： 2
再再+1后的i： 3
+666后的i为: 669
第三个defer 2
第二个defer 1
第一个defer 0
```
<br>

如果取消三处`k--`的注释, 输出为:

```go
+1后的i： 1
再+1后的i： 2
再再+1后的i： 3
+666后的i为: 669
第三个defer 1
第二个defer 0
第一个defer -1
```

<br>

等同于:

```go
package main

import "fmt"

func main() {
	foo3()
}

func foo3() {
	i := 0
	defer f1(i)

	i++
	fmt.Println("+1后的i：", i)


	defer f2(i)

	i++
	fmt.Println("再+1后的i：", i)

	defer f3(i)
	i++
	fmt.Println("再再+1后的i：", i)

	i = i + 666

	fmt.Println("+666后的i为:", i)

}

func f1(k int) {
	k--
	fmt.Println("第一个defer", k)
}

func f2(k int) {
	k--
	fmt.Println("第二个defer", k)
}

func f3(k int) {
	k--
	fmt.Println("第三个defer", k)
}
```

defer指定的函数的参数在 defer 时确定，更深层次的原因是Go语言都是值传递。




<br>


#### 情形5! (传递指针参数!`---`传递参数给defer后面的函数, defer内外同时操作该参数)

<br>

```go
package main

import "fmt"

func main() {

	foo5()
}

func foo5() {
	i := 0
	defer func(k *int) {
		fmt.Println("第一个defer", *k)
	}(&i)

	i++
	fmt.Println("+1后的i：", i)

	defer func(k *int) {
		fmt.Println("第二个defer", *k)
	}(&i)

	i++
	fmt.Println("再+1后的i：", i)

	defer func(k *int) {
		fmt.Println("第三个defer", *k)
	}(&i)

	i++
	fmt.Println("再再+1后的i：", i)

	i = i + 666

	fmt.Println("+666后的i为:", i)
}

```
输出为:

```go
+1后的i： 1
再+1后的i： 2
再再+1后的i： 3
+666后的i为: 669
第三个defer 669
第二个defer 669
第一个defer 669
```


<br>

作如下修改:

```go
package main

import "fmt"

func main() {

	foo5()
}

func foo5() {
	i := 0
	defer func(k *int) {
		(*k)--
		fmt.Println("第一个defer", *k)
	}(&i)

	i++
	fmt.Println("+1后的i：", i)

	defer func(k *int) {
		(*k)--
		fmt.Println("第二个defer", *k)
	}(&i)

	i++
	fmt.Println("再+1后的i：", i)

	defer func(k *int) {
		(*k)--
		fmt.Println("第三个defer", *k)
	}(&i)

	i++
	fmt.Println("再再+1后的i：", i)

	i = i + 666

	fmt.Println("+666后的i为:", i)
}
```


输出为:

```go
+1后的i： 1
再+1后的i： 2
再再+1后的i： 3
+666后的i为: 669
第三个defer 668
第二个defer 667
第一个defer 666
```

#### 总结一下

**即 如果传参进defer后面的函数(无论是闭包(i)方式还是字方法f(i)方式,或是直接跟如fmt.Println(i)),defer回溯时均以以当时传参时i的值去计算**

**反之,defer回溯时,以最后i的值带入计算;(参考下面的例子).**




参考:

[Go面试题答案与解析](https://yushuangqi.com/blog/2017/golang-mian-shi-ti-da-an-yujie-xi.html)

<br>


---

<br>


### 几种写法之间的归类与区别

<br>


```go
package main

import "fmt"

func main() {
	rs := foo6()
	fmt.Println("in main func:", rs)
}

func foo6() int {
	i := 0
	defer fmt.Println("in defer :", i)

	//defer func() {
	//	fmt.Println("in defer :", i)
	//}()

	i = 1000
	fmt.Println("in foo:", i)
	return i+24
}

```

输出为:

```go
in foo: 1000
in defer : 0
in main func: 1024
```

<br>

如果改为:

```go
package main

import "fmt"

func main() {
	rs := foo6()
	fmt.Println("in main func:", rs)
}

func foo6() int {
	i := 0
	//defer fmt.Println("in defer :", i)
	defer func() {
		fmt.Println("in defer :", i)
	}()

	i = 1000
	fmt.Println("in foo:", i)
	return i+24
}
```

输出为:

```go
in foo: 1000
in defer : 1000
in main func: 1024
```

<br>

也可见,

```go
defer fmt.Println("in defer :", i)
```
相当于

```go
defer func(k int) {
		fmt.Println(k)
    }(i)
```
或

```go
func f(k int){
	fmt.Println(k)
}
```

这时的参数,都是传递时的值
<br>

而如

```go
	defer func() {
		fmt.Println("in defer :", i)
    }()
```
这时的参数,为最后return之前那一刻的值


<br>

---

<br>


### defer会影响返回值吗?


<br>




函数的return value 不是原子操作, 在编译器中实际会被分解为两部分：`返回值赋值` 和 `return` 。而defer刚好被插入到末尾的return前执行(**即defer介于二者之间**)。故可以在defer函数中修改返回值



```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println(doubleScore(0))    //0
	fmt.Println(doubleScore(20.0)) //40
	fmt.Println(doubleScore(50.0)) //50
}
func doubleScore(source float32) (rs float32) {
	defer func() {
		if rs < 1 || rs >= 100 {
			//将影响返回值
			rs = source
		}
	}()
	rs = source * 2
	return
	//或者
	//return source * 2
}
```

输出为:
```go
0
40
50
```


再如:

```go
func main() {

    fmt.Println("foo return :", foo2())

}

func foo() map[string]string {

    m := map[string]string{}

    defer func() {
        m["a"] = "b"
    }()

    return m
}
```


输出为:

```go
foo return : map[a:b]
```


又如:

```go
package main


import "fmt"

func main() {
	fmt.Println("foo return :", foo())
}

func foo() int {
	i := 0

	defer func() {
		i = 10086
	}()

	return i + 5
}

```

输出为:

```go
foo return : 5
```

若作如下修改:


```go

func foo() (i int) {
	i = 0
	defer func() {
		i = 10086
	}()

	return i + 5
}
```

则返回为:

```go
foo return : 10086
```


<font color="red"><b>return之后的语句先执行，defer后的语句后执行</b></font>


将`return value`拆解为两步: 确定value值,然后return..即如果return 后面是个方法或者复杂表达式,且有某个值i,会先计算.完成后defer再执行,如果defer里面也有对i的改动,是可以影响返回值的




(给函数返回值申明变量名, 这时, 变量的内存空间空间是在函数执行前就开辟出来的，且**该变量的作用域为整个函数**,return时只是返回这个变量的内存空间的内容，因此defer能够改变返回值)




defer不影响返回值，除非是map、slice和chan这三种引用类型，或者返回值定义了变量名

参考:

[Golang研学：如何掌握并用好defer](https://segmentfault.com/a/1190000019063371#comment-area)--存疑("引用传递"那里明显错误)


权威参考:

[Golang中的Defer必掌握的7知识点](https://mp.weixin.qq.com/s/nU5GfP_vgLmhmdHiCXmuJA)


Go语言,C语言,都是`值拷贝`语言,当返回一个变量时,如`return i`,返回值是没有名称的(匿名的),不是把i返回给上层(方法/函数),而是把i的值返回给上层的一个内存.

(上层调用方(如main)得到的这个变量(即子方法的返回值)的内存块,和子方法的这个i的内存,不是一个...所以经过值拷贝后,main里得到的值不是i本身.)

当return时,上层会拷贝一份i,存在内存块里(如记为变量p),main里就得到了p里的值..然后子方法触发defer,只会修改局部变量i,而不会影响p, 进而也不会影响main里得到的值


而如果指定了返回值的名称如t,则上层存储变量值的内存p,始终会与t的值一致.进而就可以修改



而map/slice/chan为引用类型,即返回的是一个指向内存地址的指针,故而:复制了一份返回值的地址`---`触发了defer,修改了返回值`---`main中根据返回值地址得到的值,也会发生改变


特别感谢[刘丹冰Aceld](https://mp.weixin.qq.com/s/nU5GfP_vgLmhmdHiCXmuJA)大神的不吝指点与赐教



---


关于"值传递和引用传递",可参考:

[Java 到底是值传递还是引用传递？](https://www.zhihu.com/question/31203609)

[C++ 值传递、指针传递、引用传递详解](https://www.cnblogs.com/yanlingyin/archive/2011/12/07/2278961.html)


> 值传递：
<br>
形参是实参的拷贝，改变形参的值并不会影响外部实参的值。从被调用函数的角度来说，值传递是单向的（实参->形参），参数的值只能传入，
<br>
不能传出。当函数内部需要修改参数，并且不希望这个改变影响调用者时，采用值传递。
<br>
指针传递：
<br>
形参为指向实参地址的指针，当对形参的指向操作时，就相当于对实参本身进行的操作
<br>
引用传递：
<br>
形参相当于是实参的“别名”，对形参的操作其实就是对实参的操作，在引用传递过程中，被调函数的形式参数虽然也作为局部变量在栈
<br>
中开辟了内存空间，但是这时存放的是由主调函数放进来的实参变量的地址。被调函数对形参的任何操作都被处理成间接寻址，即通过
<br>
栈中存放的地址访问主调函数中的实参变量。正因为如此，被调函数对形参做的任何操作都影响了主调函数中的实参变量。



---


更多扩展:

[使用 defer 还是不使用 defer?](https://colobu.com/2019/01/22/Runtime-overhead-of-using-defer-in-go/)

最新版本对此做了优化
