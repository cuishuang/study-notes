---
title: 'golang利用组合实现继承,和php或java面向对象的继承有何不同'
date: 2014-03-01 23:54:15
tags: Go
---

<br>

### <font color="#00FFFF"> 对于golang</font>


<br>


Go语言不支持继承,但支持组合.面向对象语言<font color=red>继承</font>中的部分功能,可以使用<font color=red>组合</font>来近似实现.

组合由结构体嵌套结构体来实现.

假设有两个结构体`---` 作家`author`("父类")和作品`work`("子类"),两结构体字段如下:

```go

type author struct {
	name        string //作者名
	nationality string //国籍
	gender      string
	age         int
	work //匿名(或者称为内嵌)字段，即该字段没有显式的名字。仅指明字段的类型，此时该类型就是字段的名字。切片类型在嵌套结构体时不能匿名,但结构体类型在嵌套结构体时可以匿名
}

type work struct {
	name          string //作品名
	category      string //作品类型
	completedYear int    //完笔年份
}

```

<br>

<font color=blue>author</font>结构体的字段有name, nationality, gender, age, 同时嵌套一个匿名结构体<font color=blue>work</font>。该字段表明`author`结构体是由`work`结构体组合而成。

则现在结构体`author`可以访问`work`结构体的所有成员和方法。声明一个`work`类型的变量`w`,再声明一个`author`类型的变量`a`;


```go
	var w work
	w.name = "战争与和平"
	w.category = "masterpiece"
	w.completedYear = 1869

	//等同于 
	//w := work{
	//	name:"战争与和平",
	//	category:"masterpiece",
	//	completedYear:1869,
	//}

	var a author
	a.name = "托尔斯泰"
	a.nationality = "俄国"
	a.gender = "男"
	a.age = 82
	a.work = w

	fmt.Printf("作者名为:%s,作品名为:%s", a.name, a.work.name)
	fmt.Println("\n")

	fmt.Println(a.category)
	fmt.Println(a.work.category)

```

<font color="#FF8C00">一旦一个结构体嵌套另外一个结构体，Go访问嵌套结构体成员时，就好像是在访问自己结构体成员。也就是说`a.work.category`可以用`a.category`来代替。</font>

<font color="#F4A460">但当两个结构体都有一个相同字段时,如此处的`name`,则访问"子结构体"`work`中的name属性,需要`a.work.category`,中间的`.work`不能省却;</font>

<br>

*上述代码输出:*

```
作者名为:战争与和平,作品名为:战争与和平

masterpiece
masterpiece
```

<br>

---

<br>


**结构体的同名成员(字段)在嵌套时如此,那同名的方法呢?**

<br>

见如下代码:

```go
package main

import "fmt"

type author struct {
	name        string //作者名
	nationality string //国籍
	gender      string
	age         int
	work //匿名(或者称为内嵌)字段，即该字段没有显式的名字。仅指明字段的类型，此时该类型就是字段的名字。切片类型在嵌套结构体时不能匿名,但结构体类型在嵌套结构体时可以匿名
}

type work struct {
	name          string //作品名
	category      string //作品类型
	completedYear int    //完笔年份
}

func (aa author) printSomething() {
	fmt.Println("我是作家")
}

func (ww work) printSomething() {
	fmt.Println("我是著作")
}

func (ww work) tellYouSomething() {
	fmt.Println("作者将“战争”与“和平”的两种生活、两条线索交叉描写，构成一部百科全书式的壮阔史诗。")
}

func main() {

	var w work
	w.name = "战争与和平"
	w.category = "masterpiece"
	w.completedYear = 1869

	//等同于
	//w := work{
	//	name:"战争与和平",
	//	category:"masterpiece",
	//	completedYear:1869,
	//}

	var a author
	a.name = "托尔斯泰"
	a.nationality = "俄国"
	a.gender = "男"
	a.age = 82
	a.work = w

	a.printSomething()

	a.work.printSomething()

	a.tellYouSomething()

	a.work.tellYouSomething()
}

```

<br>

输出为:
```
我是作家
我是著作
作者将“战争”与“和平”的两种生活、两条线索交叉描写，构成一部百科全书式的壮阔史诗。
作者将“战争”与“和平”的两种生活、两条线索交叉描写，构成一部百科全书式的壮阔史诗。
```

<br>

可见对方法同样如此.

另注:

golang中的方法和函数与其他编程语言中的同名概念有所不同.官方定义是`方法是包含了接收者的函数`;

<font color="#808000">Golang 不是一个纯粹的面向对象的编程语言，它不支持类。因此通过在一个类型上建立方法来实现与 class 相似的行为。

同名方法可以定义在不同的类型上，但是 Golang 不允许同名函数。假设有两个结构体 Square 和 Circle。在 Square 和 Circle 上定义同名的方法是合法的。</font>


[更多可点击](https://www.cnblogs.com/flash55/p/10546501.html),尤其是其中"值接收者和指针接收者"值得一看

<br>


---

<br>

参考 :

[Golang 入门 : 结构体(struct)](https://www.cnblogs.com/sparkdev/archive/2019/04/25/10761825.html)

[Golang教程第27节--组合取代继承](https://blog.csdn.net/cbmljs/article/details/83052370)

[golang 组合 继承](https://www.baidu.com/s?ie=utf-8&f=3&rsv_bp=1&srcqid=3272563739082068518&tn=50000021_hao_pg&wd=golang%20%E7%BB%84%E5%90%88%20%E7%BB%A7%E6%89%BF&oq=go%25E9%259D%25A2%25E7%259B%25B8%25E7%25BB%2584%25E5%2590%2588&rsv_pq=b75749c00021cc73&rsv_t=214aUnSm3JR30bJfhBXMRzPiv2RHYxTVepaHngsYVlaiFJOFi%2BpjXHSpa1iEH2Zrn5jipAbP&rqlang=cn&rsv_enter=1&inputT=4346&rsv_sug3=32&rsv_sug1=19&rsv_sug7=100&rsv_sug2=1&prefixsug=golang%25E9%259D%25A2%25E7%259B%25B8%25E7%25BB%2584%25E5%2590%2588&rsp=1&rsv_sug4=5395)

<br>


---

<br>

###  <font color="#00FFFF"> 对于php</font>


<br>


```PHP
class author {

    var $name = "托尔斯泰" ;
    var $nationality = "俄国";
    var $gender = "男";
    var $age = 82;

    public function printSomething($one,$two){

        echo "我是作家\n";
        var_dump($one);
        var_dump($two);
    }

    public function father(){
        echo "我是父类中的方法\n";
    }

}

class work extends author {

    var $name = "战争与和平";
    var $category = "masterpiece";
    var $completedYear = 1869;

    public function printSomething($one,$two){

        echo "我是著作\n";
        var_dump($one);
        return "666";
    }

    public function children(){
        echo "我是子类中的方法\n";
    }

}


$cs = new work();

$cs->father();
$cs->children();

$cs->printSomething("shuang","hahaha");

echo $cs->name;
echo PHP_EOL;

```


<br>


输出结果为:


```
我是父类中的方法
我是子类中的方法
我是著作
string(6) "shuang"
战争与和平
```

<br>

---

<br>

### <font color="#00FFFF"> 总结</font>

<br>

<font color="brown">
 对方法的调用:
 $this->方法名();如果子类中有该方法则调用的是子类中的方法，若没有则是调用父类中的;

 在子类内部,使用parent::则始终调用的是父类中的方法。


 对变量的调用：
 $this->变量名；如果子类中有该变量则调用的是子类中的，若没有则调用的是父类中的

</font>

<br>

思考:如上代码中,如何在只实例化子类而不实例化父类的前提下,也不修改和侵入子类代码的情况下,实现调用父类中的(被子类重写的)printSomething()方法?


<br>


---

<br>

另注:自PHP5.3之后,子类重写(override)父类方法时(即在子类中重新定义一个和父类中某方法同名的新方法),子类中这个新方法的形参和父类的形参个数必须一致(因为php是弱类型语言,所以对类型是否一致不要求),对返回值无要求;

(如上面代码中,如果`class work`中的**printSomething**方法只有一个参数即printSomething($one),则在运行时会报错:

`Warning: Declaration of work::printSomething($one) should be compatible with author::printSomething($one, $two) in /Users/xxxx/xxx/xxx.php on line xx`)  



对于静态方法,情况类似

参考:

[php面向对象的重写与重载](https://www.cnblogs.com/xuan584521/p/6395217.html)

[PHP类方法重写原则](https://www.cnblogs.com/joyco773/p/6020334.html)

[php子类中如何调用父类中的变量和方法](https://www.cnblogs.com/liangdong/p/10373423.html)

[php继承父类，子类和父类中都有同名方法，实例化子类，在父类中调用这个方法，调用的是子类的](https://blog.csdn.net/zpf_nevergiveup/article/details/78977316)



---

<br>
