---
title: 'Rust vs Go:常用语法对比'
date: 2021-09-02 21:02:52
tags: Rust
---


[这个网站](https://programming-idioms.org/about#about-block-cheatsheets) 可以列出某门编程语言的常用语法，也可以对比两种语言的基本语法差别。

在此对比[Go和Rust](https://programming-idioms.org/cheatsheet/Go/Rust)

<br>

### 1. <font color="d9ed92">Print Hello World</font>

*打印Hello World*

```go
package main

import "fmt"


func main() {
	fmt.Println("Hello World")
}

```

---

```rust
fn main() {
    println!("Hello World");
}

```

> Rust 输出文字的方式主要有两种：**println!()** 和 **print!()**。这两个"函数"都是向命令行输出字符串的方法，区别仅在于前者会在输出的最后附加输出一个换行符。当用这两个"函数"输出信息的时候，第一个参数是格式字符串，后面是一串可变参数，对应着格式字符串中的"占位符"，这一点与 C 语言/ Go语言 中的 printf 函数很相似。但是，Rust 中格式字符串中的占位符不是"% + 字母"的形式，而是一对 {}。

<br>

### 2. <font color="b5e48c">Print Hello 10 times</font>

*打印10次Hello World*

```go
package main

import (
	"fmt"
)

func main() {
	for i := 0; i < 10; i++ {
		fmt.Println("Hello")
	}
}
```

---

```rust
fn main() {
    for _ in 0..10 {
        println!("Hello");
    }
}
```

or

```rust
fn main() {
    print!("{}", "Hello\n".repeat(10));
}

```



<br>

### 3. <font color="99d98c">Create a procedure</font>


> Like a function which doesn't return any value, thus has only side effects (e.g. Print to standard output)

*创建一个方法，没有返回值，打印一些内容*


```go
package main

import "fmt"

func finish(name string) {
	fmt.Println("My job here is done. Good bye " + name)
}

func main() {
	finish("Tony")
}

```

---


```rust
fn main(){
    finish("Buddy")
}

fn finish(name : &str) {
    println!("My job here is done. Goodbye {}", name);
}
```

<br>

### 4. <font color="76c893">Create a function which returns the square of an integer</font>


*创建一个函数,返回一个整数的平方*

```go
func square(x int) int {
  return x*x
}

```

---


```rust
fn square(x: u32) -> u32 {
    x * x
}

fn main() {
    let sq = square(9);

    println!("{}", sq);
}

```

<br>

### 5. <font color="52b69a">Create a 2D Point data structure</font>

> Declare a container type for two floating-point numbers x and y

*声明一个容器类型,有x、y两个浮点数*

```go
package main

import "fmt"

type Point struct {
	x, y float64
}

func main() {
	p1 := Point{}
	p2 := Point{2.1, 2.2}
	p3 := Point{
		y: 3.1,
		x: 3.2,
	}
	p4 := &Point{
		x: 4.1,
		y: 4.2,
	}

	fmt.Println(p1)
	fmt.Println(p2)
	fmt.Println(p3)
	fmt.Println(p4)
}

```

输出

```GO
{0 0}
{2.1 2.2}
{3.2 3.1}
&{4.1 4.2}

```


---

```rust
use std::fmt;

struct Point {
    x: f64,
    y: f64,
}

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

fn main() {
    let p = Point { x: 2.0, y: -3.5 };

    println!("{}", p);
}
```

or

```rust
use std::fmt;

struct Point(f64, f64);

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "({}, {})", self.0, self.1)
    }
}

fn main() {
    let p = Point(2.0, -3.5);

    println!("{}", p);
}

```

<br>

### 6. <font color="34a0a4">Iterate over list values</font>


> Do something with each item x of an array-like collection items, regardless indexes.

*遍历列表的值*

```go
for _, x := range items {
    doSomething(x)
}

```

```GO
package main

import (
	"fmt"
)

func main() {
	items := []int{11, 22, 33}

	for _, x := range items {
		doSomething(x)
	}
}

func doSomething(i int) {
	fmt.Println(i)
}
```

输出

```GO
11
22
33
```


---


```rust
fn main() {
    let items = vec![11, 22, 33];

    for x in items {
        do_something(x);
    }
}

fn do_something(n: i64) {
    println!("Number {}", n)
}
```

or

```rust
fn main() {
    let items = vec![11, 22, 33];

    items.into_iter().for_each(|x| do_something(x));
}

fn do_something(n: i64) {
    println!("Number {}", n)
}
```


<br>

### 7. <font color="168aad">Iterate over list indexes and values</font>

*遍历列表的索引和值*

```go
package main

import "fmt"

func main() {
	items := []string{
		"oranges",
		"apples",
		"bananas",
	}

	for i, x := range items {
		fmt.Printf("Item %d = %v \n", i, x)
	}
}

```

输出

```GO
Item 0 = oranges 
Item 1 = apples 
Item 2 = bananas 
```

---

```rust
fn main() {
    let items = ["a", "b", "c"];
    for (i, x) in items.iter().enumerate() {
        println!("Item {} = {}", i, x);
    }
}
```
or

```rust
fn main() {
    let items = ["a", "b", "c"];
    items.iter().enumerate().for_each(|(i, x)| {
        println!("Item {} = {}", i, x);
    });
}
```

<br>

### 8. <font color="1a759f">Initialize a new map (associative array)</font>

> Create a new map object x, and provide some (key, value) pairs as initial content.

*创建一个新的map,提供一些键值对 作为初始内容*

```go
package main

import "fmt"

func main() {
	x := map[string]int{"one": 1, "two": 2}

	fmt.Println(x)
}

```

输出

```go
map[one:1 two:2]
```

---

```rust
use std::collections::BTreeMap;

fn main() {
    let mut x = BTreeMap::new();
    x.insert("one", 1);
    x.insert("two", 2);
    
    println!("{:?}", x);
}
```

输出为：

```rs
("one", 1)
("two", 2)
```

or

```rust
use std::collections::HashMap;

fn main() {
    let x: HashMap<&str, i32> = [
        ("one", 1),
        ("two", 2),
    ].iter().cloned().collect();
    
    println!("{:?}", x);
}
```
输出为：

```rs
("two", 2)
("one", 1)
```

分 BTreeMap 和 HashMap，且都需要use进来

<br>

### 9. <font color="1e6091">Create a Binary Tree data structure</font>

> The structure must be recursive because left child and right child are binary trees too. A node has access to children nodes, but not to its parent.

*创建一个二叉树*

```go
type BinTree struct {
	Value valueType
	Left *BinTree
	Right *BinTree
}
```

```go
package main

import "fmt"

type BinTree struct {
	Value int
	Left  *BinTree
	Right *BinTree
}

func inorder(root *BinTree) {
	if root == nil {
		return
	}

	inorder(root.Left)
	fmt.Printf("%d ", root.Value)
	inorder(root.Right)
}

func main() {
	root := &BinTree{1, nil, nil}
	root.Left = &BinTree{2, nil, nil}
	root.Right = &BinTree{3, nil, nil}
	root.Left.Left = &BinTree{4, nil, nil}
	root.Left.Right = &BinTree{5, nil, nil}
	root.Right.Right = &BinTree{6, nil, nil}
	root.Left.Left.Left = &BinTree{7, nil, nil}

	inorder(root)
}
```

输出

```go
7 4 2 5 1 3 6 
```

---

```rust
struct BinTree<T> {
    value: T,
    left: Option<Box<BinTree<T>>>,
    right: Option<Box<BinTree<T>>>,
}

```

<br>

### 10. <font color="184e77">Shuffle a list</font>

> Generate a random permutation of the elements of list x

*随机排序一个list*


```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	x := []string{"a", "b", "c", "d", "e", "f", "g", "h"}

	for i := range x {
		j := rand.Intn(i + 1)
		x[i], x[j] = x[j], x[i]
	}

	fmt.Println(x)
}

```

输出

`[f e c g h a d b]`

or

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	x := []string{"a", "b", "c", "d", "e", "f", "g", "h"}

	y := make([]string, len(x))
	perm := rand.Perm(len(x))
	for i, v := range perm {
		y[v] = x[i]
	}

	fmt.Println(y)
}
```
输出

`[f h c g b a d e]`

or


```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	x := []string{"a", "b", "c", "d", "e", "f", "g", "h"}

	y := make([]string, len(x))
	perm := rand.Perm(len(x))
	for i, v := range perm {
		y[v] = x[i]
	}

	fmt.Println(y)
}

```

输出

`[f h c g b a d e]`

or

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	x := []string{"a", "b", "c", "d", "e", "f", "g", "h"}

	for i := len(x) - 1; i > 0; i-- {
		j := rand.Intn(i + 1)
		x[i], x[j] = x[j], x[i]
	}

	fmt.Println(x)
}
```
输出

`[g d a h e f c b]`

---


```rust
extern crate rand;
use rand::{Rng, StdRng};

let mut rng = StdRng::new().unwrap();
rng.shuffle(&mut x);
```

or

```rust
use rand::seq::SliceRandom;
use rand::thread_rng;

fn main() {
    let mut x = [1, 2, 3, 4, 5];
    println!("Unshuffled: {:?}", x);

    let mut rng = thread_rng();
    x.shuffle(&mut rng);

    println!("Shuffled:   {:?}", x);
}

```


<br>

### 11. <font color="f72585">Pick a random element from a list</font>

*从列表中选择一个随机元素*


```go
package main

import (
	"fmt"
	"math/rand"
)

var x = []string{"bleen", "fuligin", "garrow", "grue", "hooloovoo"}

func main() {
	fmt.Println(x[rand.Intn(len(x))])
}

```

输出

`fuligin`

or

```go
package main

import (
	"fmt"
	"math/rand"
)

type T string

func pickT(x []T) T {
	return x[rand.Intn(len(x))]
}

func main() {
	var list = []T{"bleen", "fuligin", "garrow", "grue", "hooloovoo"}
	fmt.Println(pickT(list))
}

```

输出

`fuligin`


---

```rust
use rand::{self, Rng};

fn main() {
    let x = vec![11, 22, 33];

    let choice = x[rand::thread_rng().gen_range(0..x.len())];

    println!("I picked {}!", choice);
}
```

or

```rust
use rand::seq::SliceRandom;
 
fn main() {
    let x = vec![11, 22, 33];

    let mut rng = rand::thread_rng();
    let choice = x.choose(&mut rng).unwrap();
    
    println!("I picked {}!", choice);
}
```

<br>

### 12. <font color="b5179e">Check if list contains a value</font>

> Check if list contains a value x.
list is an iterable finite container.

*检查列表中是否包含一个值*

```go
package main

import "fmt"

func Contains(list []T, x T) bool {
	for _, item := range list {
		if item == x {
			return true
		}
	}
	return false
}

type T string

func main() {
	list := []T{"a", "b", "c"}
	fmt.Println(Contains(list, "b"))
	fmt.Println(Contains(list, "z"))
}

```

输出

```go
true
false
```

---

```rust
fn main() {
    let list = [10, 40, 30];

    {
        let num = 30;

        if list.contains(&num) {
            println!("{:?} contains {}", list, num);
        } else {
            println!("{:?} doesn't contain {}", list, num);
        }
    }

    {
        let num = 42;

        if list.contains(&num) {
            println!("{:?} contains {}", list, num);
        } else {
            println!("{:?} doesn't contain {}", list, num);
        }
    }
}
```

or

```rust
fn main() {
    let list = [10, 40, 30];
    let x = 30;

    if list.iter().any(|v| v == &x) {
        println!("{:?} contains {}", list, x);
    } else {
        println!("{:?} doesn't contain {}", list, x);
    }
}
```

or

```rust
fn main() {
    let list = [10, 40, 30];
    let x = 30;

    if (&list).into_iter().any(|v| v == &x) {
        println!("{:?} contains {}", list, x);
    } else {
        println!("{:?} doesn't contain {}", list, x);
    }
}
```

<br>

### 13. <font color="7209b7">Iterate over map keys and values</font>

> Access each key k with its value x from an associative array mymap, and print them


*遍历关联数组中的每一对 k-v， 并打印出它们*

```go
package main

import "fmt"

func main() {
	mymap := map[string]int{
		"one":   1,
		"two":   2,
		"three": 3,
		"four":  4,
	}

	for k, x := range mymap {
		fmt.Println("Key =", k, ", Value =", x)
	}
}
```

输出

```go
Key = two , Value = 2
Key = three , Value = 3
Key = four , Value = 4
Key = one , Value = 1
```

---


```rust
use std::collections::BTreeMap;

fn main() {
    let mut mymap = BTreeMap::new();
    mymap.insert("one", 1);
    mymap.insert("two", 2);
    mymap.insert("three", 3);
    mymap.insert("four", 4);

    for (k, x) in &mymap {
        println!("Key={key}, Value={val}", key = k, val = x);
    }
}
```

<br>

### 14. <font color="560bad">Pick uniformly a random floating point number in [a..b)</font>

> Pick a random number greater than or equals to a, strictly inferior to b. Precondition : a < b.

*选出一个随机的浮点数，大于或等于a，严格小于b，且a< b*

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	x := pick(-2.0, 6.5)
	fmt.Println(x)
}

func pick(a, b float64) float64 {
	return a + (rand.Float64() * (b - a))
}
```

输出

`3.1396124478267664`

---

```rust
extern crate rand;
use rand::{thread_rng, Rng};

fn main() {
    let (a, b) = (1.0, 3.0);
    let c = thread_rng().gen_range(a..b);
    println!("{}", c);
}
```

<br>

### 15. <font color="3f37c9">Pick uniformly a random integer in [a..b]</font>

> Pick a random integer greater than or equals to a, inferior or equals to b. Precondition : a < b.

*选出一个随机的整数，大于或等于a，小于或等于b，且a< b*

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	x := pick(3, 7)

	// Note that in the Go Playground, time and random don't change very often.
	fmt.Println(x)
}

func pick(a, b int) int {
	return a + rand.Intn(b-a+1)
}

```

输出

`4`

---

```rust
fn pick(a: i32, b: i32) -> i32 {
    let between = Range::new(a, b);
    let mut rng = rand::thread_rng();
    between.ind_sample(&mut rng)
}
```

or

```rust
use rand::distributions::Distribution;
use rand::distributions::Uniform;

fn main() {
    let (a, b) = (3, 5);

    let x = Uniform::new_inclusive(a, b).sample(&mut rand::thread_rng());

    println!("{}", x);
}
```


<br>

### 17. <font color="4361ee">Create a Tree data structure</font>

> The structure must be recursive. A node may have zero or more children. A node has access to children nodes, but not to its parent.

*创建树数据结构,
该结构必须是递归的。一个节点可以有零个或多个子节点,节点可以访问子节点，但不能访问其父节点*

```go
type Tree struct {
	Key keyType
	Deco valueType
	Children []*Tree
}
```

```go
package main

import "fmt"

type Tree struct {
	Key      key
	Deco     value
	Children []*Tree
}

type key string
type value string

func (t *Tree) String() string {
	str := "("
	str += string(t.Deco)
	if len(t.Children) == 0 {
		return str + ")"
	}
	str += " ("
	for _, child := range t.Children {
		str += child.String()
	}
	str += "))"
	return str
}

func (this *Tree) AddChild(x key, v value) *Tree {
	child := &Tree{Key: x, Deco: v}
	this.Children = append(this.Children, child)
	return child
}

func main() {
	tree := &Tree{Key: "Granpa", Deco: "Abraham"}
	subtree := tree.AddChild("Dad", "Homer")
	subtree.AddChild("Kid 1", "Bart")
	subtree.AddChild("Kid 2", "Lisa")
	subtree.AddChild("Kid 3", "Maggie")

	fmt.Println(tree)
}

```


输出

`(Abraham ((Homer ((Bart)(Lisa)(Maggie)))))`




---

```rust
use std::vec;

struct Node<T> {
    value: T,
    children: Vec<Node<T>>,
}

impl<T> Node<T> {
    pub fn dfs<F: Fn(&T)>(&self, f: F) {
       self.dfs_helper(&f);
    }

    fn dfs_helper<F: Fn(&T)>(&self, f: &F) {
        (f)(&self.value);
        for child in &self.children {
            child.dfs_helper(f);
        }
    }
}


fn main() {
    let t: Node<i32> = Node {
        children: vec![
            Node {
                children: vec![
                    Node {
                        children: vec![],
                        value: 14
                    }
                ],
                value: 28
            },
            Node {
                children: vec![],
                value: 80
            }
        ],
        value: 50
    };

    t.dfs(|node| { println!("{}", node); });
}
```

输出：

```rust
50
28
14
80
```

<br>

### 18. <font color="4895ef">Depth-first traversing of a tree</font>


> Call a function f on every node of a tree, in depth-first prefix order

*树的深度优先遍历。按照深度优先的前缀顺序，在树的每个节点上调用函数f*

```go
package main

import . "fmt"

func (t *Tree) Dfs(f func(*Tree)) {
	if t == nil {
		return
	}
	f(t)
	for _, child := range t.Children {
		child.Dfs(f)
	}
}

type key string
type value string

type Tree struct {
	Key      key
	Deco     value
	Children []*Tree
}

func (this *Tree) AddChild(x key, v value) {
	child := &Tree{Key: x, Deco: v}
	this.Children = append(this.Children, child)
}

func NodePrint(node *Tree) {
	Printf("%v (%v)\n", node.Deco, node.Key)
}

func main() {
	tree := &Tree{Key: "Granpa", Deco: "Abraham"}
	tree.AddChild("Dad", "Homer")
	tree.Children[0].AddChild("Kid 1", "Bart")
	tree.Children[0].AddChild("Kid 2", "Lisa")
	tree.Children[0].AddChild("Kid 3", "Maggie")

	tree.Dfs(NodePrint)
}

```

输出

```go
Abraham (Granpa)
Homer (Dad)
Bart (Kid 1)
Lisa (Kid 2)
Maggie (Kid 3)
```


---

```rust
use std::vec;

struct Tree<T> {
    children: Vec<Tree<T>>,
    value: T
}

impl<T> Tree<T> {
    pub fn new(value: T) -> Self{
        Tree{
            children: vec![],
            value
        }
    }

    pub fn dfs<F: Fn(&T)>(&self, f: F) {
       self.dfs_helper(&f);
    }

    fn dfs_helper<F: Fn(&T)>(&self, f: &F) {
        (f)(&self.value);
        for child in &self.children {
            child.dfs_helper(f);
        }
    }
}


fn main() {
    let t: Tree<i32> = Tree {
        children: vec![
            Tree {
                children: vec![
                    Tree {
                        children: vec![],
                        value: 14
                    }
                ],
                value: 28
            },
            Tree {
                children: vec![],
                value: 80
            }
        ],
        value: 50
    };

    t.dfs(|node| { println!("{}", node); });
}
```

输出：

```rust
50
28
14
80
```


<br>

### 19. <font color="4895ef">Reverse a list</font>

> Reverse the order of the elements of list x.
This may reverse "in-place" and destroy the original ordering.

*反转链表*

```go
package main

import "fmt"

func main() {

	s := []int{5, 2, 6, 3, 1, 4}

	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		s[i], s[j] = s[j], s[i]
	}

	fmt.Println(s)
}

```

输出

`[4 1 3 6 2 5]`

---


```rust
fn main() {
    let x = vec!["Hello", "World"];
    let y: Vec<_> = x.iter().rev().collect();
    println!("{:?}", y);
}
```

输出：

```rust
["World", "Hello"]
```

or

```rust
fn main() {
    let mut x = vec![1,2,3];
    x.reverse();
    println!("{:?}", x);
}
```
输出:

```rust
[3, 2, 1]
```

<br>

### 20. <font color="14213d">Return two values</font>

>Implement a function search which looks for item x in a 2D matrix m.
Return indices i, j of the matching cell.
Think of the most idiomatic way in the language to return the two values at the same time.


*实现在2D矩阵m中寻找元素x，返回匹配单元格的索引 i，j*

```go
package main

import "fmt"

func search(m [][]int, x int) (bool, int, int) {
	for i := range m {
		for j, v := range m[i] {
			if v == x {
				return true, i, j
			}
		}
	}
	return false, 0, 0
}

func main() {
	matrix := [][]int{
		{1, 2, 3},
		{4, 5, 6},
		{7, 8, 9},
	}
	for x := 1; x <= 11; x += 2 {
		found, i, j := search(matrix, x)
		if found {
			fmt.Printf("matrix[%v][%v] == %v \n", i, j, x)
		} else {
			fmt.Printf("Value %v not found. \n", x)
		}
	}
}
```

输出

```go
matrix[0][0] == 1 
matrix[0][2] == 3 
matrix[1][1] == 5 
matrix[2][0] == 7 
matrix[2][2] == 9 
Value 11 not found. 
```

---

```rust
fn search<T: Eq>(m: &Vec<Vec<T>>, x: &T) -> Option<(usize, usize)> {
    for (i, row) in m.iter().enumerate() {
        for (j, column) in row.iter().enumerate() {
            if *column == *x {
                return Some((i, j));
            }
        }
    }
    
    None
}

fn main() {
    let a = vec![
        vec![0, 11],
        vec![22, 33],
        vec![44, 55],
    ];
    
    let hit = search(&a, &33);
    
    println!("{:?}", hit);
}
```

输出：

```rust
Some((1, 1))
```

<br>




