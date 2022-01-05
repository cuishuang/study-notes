---
title: 'Rust vs Go:常用语法对比(7)'
date: 2021-09-08 19:43:55
tags: Rust
---


<br>

### 121. <font color="0c0a3e">UDP listen and read</font>

>  Listen UDP traffic on port p and read 1024 bytes into buffer b.

*听端口p上的UDP流量，并将1024字节读入缓冲区b。*

```go
import (
    "fmt"
    "net"
    "os"
)
ServerAddr,err := net.ResolveUDPAddr("udp",p)
if err != nil {
	return err
}
ServerConn, err := net.ListenUDP("udp", ServerAddr)
if err != nil {
	return err
}
defer ServerConn.Close()
n,addr,err := ServerConn.ReadFromUDP(b[:1024])
if err != nil {
	return err
}
if n<1024 {
	return fmt.Errorf("Only %d bytes could be read.", n)
}
```

---

```rust
use std::net::UdpSocket;
let mut b = [0 as u8; 1024];
let sock = UdpSocket::bind(("localhost", p)).unwrap();
sock.recv_from(&mut b).unwrap();
```

<br>


### 122. <font color="0c0a3e">Declare enumeration</font>

> Create an enumerated type Suit with 4 possible values SPADES, HEARTS, DIAMONDS, CLUBS.


*声明枚举值*

```go
package main

import (
	"fmt"
)

type Suit int

const (
  Spades Suit = iota
  Hearts
  Diamonds
  Clubs
)

func main() {
	fmt.Printf("Hearts has type %T and value %d", Hearts, Hearts)
}

```

`Hearts has type main.Suit and value 1`

---

```rust
enum Suit {
    Spades,
    Hearts,
    Diamonds,
    Clubs,
}

fn main() {
    let _x = Suit::Diamonds;
}
```



<br>

### 123. <font color="0c0a3e"> Assert condition</font>

> Verify that predicate isConsistent returns true, otherwise report assertion violation.
Explain if the assertion is executed even in production environment or not.


*断言条件*

```go
package main

import "fmt"

//
// The code may look fine, but
// obviously we have a bug.
//

func main() {
	salary = 65000
	employees = 120000
	totalPayroll = salary * employees

	if !isConsistent() {
		panic("State consistency violated")
	}
	fmt.Println("Everything fine")
}

var salary int32
var employees int32
var totalPayroll int32

func isConsistent() bool {
	return salary >= 0 &&
		employees >= 0 &&
		totalPayroll >= 0
}
```

---

```rust
fn main() {
    // i is odd
    let i = 23687;
    let ii = i * i;
    let is_consistent = ii % 2 == 1;

    // i*i must be odd
    assert!(is_consistent);

    println!("Cool.")
}
```

`Cool.`


<br>

### 124. <font color="0c0a3e"> Binary search for a value in sorted array</font>

> Write function binarySearch which returns the index of an element having value x in sorted array a, or -1 if no such element.


*排序数组中值的二分搜索法*

**二分查找**

```go
package main

import "fmt"

func binarySearch(a []T, x T) int {
	imin, imax := 0, len(a)-1
	for imin <= imax {
		imid := (imin + imax) / 2
		switch {
		case a[imid] == x:
			return imid
		case a[imid] < x:
			imin = imid + 1
		default:
			imax = imid - 1
		}
	}
	return -1
}

type T int

func main() {
	a := []T{-2, -1, 0, 1, 1, 1, 6, 8, 8, 9, 10}
	for x := T(-5); x <= 15; x++ {
		i := binarySearch(a, x)
		if i == -1 {
			fmt.Println("Value", x, "not found")
		} else {
			fmt.Println("Value", x, "found at index", i)
		}
	}
}

```


or


```go
package main

import (
	"fmt"
	"sort"
)

func binarySearch(a []int, x int) int {
	i := sort.SearchInts(a, x)
	if i < len(a) && a[i] == x {
		return i
	}
	return -1
}

func main() {
	a := []int{-2, -1, 0, 1, 1, 1, 6, 8, 8, 9, 10}
	for x := -5; x <= 15; x++ {
		i := binarySearch(a, x)
		if i == -1 {
			fmt.Println("Value", x, "not found")
		} else {
			fmt.Println("Value", x, "found at index", i)
		}
	}
}

```


or


```go
package main

import (
	"fmt"
	"sort"
)

func binarySearch(a []T, x T) int {
	f := func(i int) bool { return a[i] >= x }
	i := sort.Search(len(a), f)
	if i < len(a) && a[i] == x {
		return i
	}
	return -1
}

type T int

func main() {
	a := []T{-2, -1, 0, 1, 1, 1, 6, 8, 8, 9, 10}
	for x := T(-5); x <= 15; x++ {
		i := binarySearch(a, x)
		if i == -1 {
			fmt.Println("Value", x, "not found")
		} else {
			fmt.Println("Value", x, "found at index", i)
		}
	}
}

```

---

```rust

```

<br>

### 125. <font color="0c0a3e">Measure function call duration</font>


> measure the duration t, in nano seconds, of a call to the function foo. Print this duration.


*函数调用时间*

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t1 := time.Now()
	foo()
	t := time.Since(t1)
	ns := int64(t / time.Nanosecond)

	// Note that the clock is fixed in the Playground, so the resulting duration is always zero
	fmt.Printf("%dns\n", ns)
}

func foo() {
	fmt.Println("Hello")
}

```


```go
Hello
0ns
```


or

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t1 := time.Now()
	foo()
	t := time.Since(t1)
	ns := t.Nanoseconds()
	fmt.Printf("%dns\n", ns)
}

func foo() {
	fmt.Println("Hello")
}
```


```go
Hello
0ns
```




---

```rust
use std::time::{Duration, Instant};
let start = Instant::now();
foo();
let duration = start.elapsed();
println!("{}", duration);
```

<br>

### 126. <font color="0c0a3e">Multiple return values</font>


> 	Write a function foo that returns a string and a boolean value.

*多个返回值*


```go
package main

import (
	"fmt"
)

func main() {
	s, b := foo()
	fmt.Println(s, b)
}

func foo() (string, bool) {
	return "Too good to be", true
}

```

`Too good to be true`

---

```rust
fn foo() -> (String, bool) {
    (String::from("bar"), true)
}

fn main() {
    println!("{:?}", foo());
}
```

`("bar", true)`

<br>


### 128. <font color="0c0a3e">Breadth-first traversing of a tree</font>


> Call a function f on every node of a tree, in breadth-first prefix order

*树的广度优先遍历*

```go
package main

import "fmt"

func (root *Tree) Bfs(f func(*Tree)) {
	if root == nil {
		return
	}
	queue := []*Tree{root}
	for len(queue) > 0 {
		t := queue[0]
		queue = queue[1:]
		f(t)
		queue = append(queue, t.Children...)
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
	fmt.Printf("%v (%v)\n", node.Key, node.Deco)
}

func main() {
	tree := &Tree{Key: "World", Deco: "Our planet"}
	tree.AddChild("Europe", "A continent")
	tree.Children[0].AddChild("Germany", "A country")
	tree.Children[0].AddChild("Ireland", "A country")
	tree.Children[0].AddChild("Mediterranean Sea", "A sea")
	tree.AddChild("Asia", "A continent")
	tree.Children[0].AddChild("Japan", "A country")
	tree.Children[0].AddChild("Thailand", "A country")

	tree.Bfs(NodePrint)
}

```


```go
World (Our planet)
Europe (A continent)
Asia (A continent)
Germany (A country)
Ireland (A country)
Mediterranean Sea (A sea)
Japan (A country)
Thailand (A country)
```


---

```rust
use std::collections::VecDeque;

struct Tree<V> {
    children: Vec<Tree<V>>,
    value: V
}

impl<V> Tree<V> {
    fn bfs(&self, f: impl Fn(&V)) {
        let mut q = VecDeque::new();
        q.push_back(self);

        while let Some(t) = q.pop_front() {
            (f)(&t.value);
            for child in &t.children {
                q.push_back(child);
            }
        }
    }
}

fn main() {
    let t = Tree {
        children: vec![
            Tree {
                children: vec![
                    Tree { children: vec![], value: 5 },
                    Tree { children: vec![], value: 6 }
                ],
                value: 2
            },
            Tree { children: vec![], value: 3 },
            Tree { children: vec![], value: 4 },
        ],
        value: 1
    };
    t.bfs(|v| println!("{}", v));
}
```

```rust
1
2
3
4
5
6
```


<br>

### 129. <font color="0c0a3e">Breadth-first traversing in a graph</font>


> Call a function f on every vertex accessible from vertex start, in breadth-first prefix order


*图的广度优先遍历*

```go
package main

import "fmt"

func (start *Vertex) Bfs(f func(*Vertex)) {
	queue := []*Vertex{start}
	seen := map[*Vertex]bool{start: true}
	for len(queue) > 0 {
		v := queue[0]
		queue = queue[1:]
		f(v)
		for next, isEdge := range v.Neighbours {
			if isEdge && !seen[next] {
				queue = append(queue, next)
				seen[next] = true
			}
		}
	}
}

type Vertex struct {
	Id         int
	Label      string
	Neighbours map[*Vertex]bool
}

type Graph []*Vertex

func NewVertex(id int, label string) *Vertex {
	return &Vertex{
		Id:         id,
		Label:      label,
		Neighbours: make(map[*Vertex]bool),
	}
}

func (v *Vertex) AddNeighbour(w *Vertex) {
	v.Neighbours[w] = true
}

func VertexPrint(v *Vertex) {
	fmt.Printf("%v (%v)\n", v.Id, v.Label)
}

func main() {
	// Some cities
	london := NewVertex(0, "London")
	ny := NewVertex(1, "New York City")
	berlin := NewVertex(2, "Berlin")
	paris := NewVertex(3, "Paris")
	tokyo := NewVertex(4, "Tokyo")

	g := Graph{
		london,
		ny,
		berlin,
		paris,
		tokyo,
	}
	_ = g

	london.AddNeighbour(paris)
	london.AddNeighbour(ny)
	ny.AddNeighbour(london)
	ny.AddNeighbour(paris)
	ny.AddNeighbour(tokyo)
	tokyo.AddNeighbour(paris)
	paris.AddNeighbour(tokyo)
	paris.AddNeighbour(berlin)

	london.Bfs(VertexPrint)
}

```


```go
0 (London)
3 (Paris)
1 (New York City)
2 (Berlin)
4 (Tokyo)
```

---

```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

struct Vertex<V> {
    value: V,
    neighbours: Vec<Weak<RefCell<Vertex<V>>>>,
}

type RcVertex<V> = Rc<RefCell<Vertex<V>>>;

struct Graph<V> {
    vertices: Vec<RcVertex<V>>,
}

impl<V> Graph<V> {
    fn new() -> Self {
        Graph { vertices: vec![] }
    }
    
    fn new_vertex(&mut self, value: V) -> RcVertex<V> {
        self.add_vertex(Vertex { value, neighbours: Vec::new() })
    }
    
    fn add_vertex(&mut self, v: Vertex<V>) -> RcVertex<V> {
        let v = Rc::new(RefCell::new(v));
        self.vertices.push(Rc::clone(&v));
        v
    }
    
    fn add_edge(&mut self, v1: &RcVertex<V>, v2: &RcVertex<V>) {
        v1.borrow_mut().neighbours.push(Rc::downgrade(&v2));
        v2.borrow_mut().neighbours.push(Rc::downgrade(&v1));
    }

    fn bft(start: RcVertex<V>, f: impl Fn(&V)) {
        let mut q = vec![start];
        let mut i = 0;
        while i < q.len() {
            let v = Rc::clone(&q[i]);
            i += 1;
            (f)(&v.borrow().value);
            for n in &v.borrow().neighbours {
                let n = n.upgrade().expect("Invalid neighbour");
                if q.iter().all(|v| v.as_ptr() != n.as_ptr()) {
                    q.push(n);
                }
            }
        }
    }
}

fn main() {
    let mut g = Graph::new();
    
    let v1 = g.new_vertex(1);
    let v2 = g.new_vertex(2);
    let v3 = g.new_vertex(3);
    let v4 = g.new_vertex(4);
    let v5 = g.new_vertex(5);
    
    g.add_edge(&v1, &v2);
    g.add_edge(&v1, &v3);
    g.add_edge(&v1, &v4);
    g.add_edge(&v2, &v5);
    g.add_edge(&v3, &v4);
    g.add_edge(&v4, &v5);
    
    Graph::bft(v1, |v| println!("{}", v));
}
```

```rust
1
2
3
4
5
```


<br>

### 130. <font color="0c0a3e">Depth-first traversing in a graph</font>

> Call a function f on every vertex accessible for vertex v, in depth-first prefix order

*图的深度优先遍历*

```go
package main

import "fmt"

func (v *Vertex) Dfs(f func(*Vertex), seen map[*Vertex]bool) {
	seen[v] = true
	f(v)
	for next, isEdge := range v.Neighbours {
		if isEdge && !seen[next] {
			next.Dfs(f, seen)
		}
	}
}

type Vertex struct {
	Id         int
	Label      string
	Neighbours map[*Vertex]bool
}

type Graph []*Vertex

func NewVertex(id int, label string) *Vertex {
	return &Vertex{
		Id:         id,
		Label:      label,
		Neighbours: make(map[*Vertex]bool),
	}
}

func (v *Vertex) AddNeighbour(w *Vertex) {
	v.Neighbours[w] = true
}

func VertexPrint(v *Vertex) {
	fmt.Printf("%v (%v)\n", v.Id, v.Label)
}

func main() {
	// Some cities
	london := NewVertex(0, "London")
	ny := NewVertex(1, "New York City")
	berlin := NewVertex(2, "Berlin")
	paris := NewVertex(3, "Paris")
	tokyo := NewVertex(4, "Tokyo")

	g := Graph{
		london,
		ny,
		berlin,
		paris,
		tokyo,
	}
	_ = g

	london.AddNeighbour(paris)
	london.AddNeighbour(ny)
	ny.AddNeighbour(london)
	ny.AddNeighbour(paris)
	ny.AddNeighbour(tokyo)
	tokyo.AddNeighbour(paris)
	paris.AddNeighbour(tokyo)
	paris.AddNeighbour(berlin)

	alreadySeen := map[*Vertex]bool{}
	london.Dfs(VertexPrint, alreadySeen)
}

```

```go
0 (London)
3 (Paris)
4 (Tokyo)
2 (Berlin)
1 (New York City)
```

---

```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

struct Vertex<V> {
    value: V,
    neighbours: Vec<Weak<RefCell<Vertex<V>>>>,
}

type RcVertex<V> = Rc<RefCell<Vertex<V>>>;

struct Graph<V> {
    vertices: Vec<RcVertex<V>>,
}

impl<V> Graph<V> {
    fn new() -> Self {
        Graph { vertices: vec![] }
    }
    
    fn new_vertex(&mut self, value: V) -> RcVertex<V> {
        self.add_vertex(Vertex { value, neighbours: Vec::new() })
    }
    
    fn add_vertex(&mut self, v: Vertex<V>) -> RcVertex<V> {
        let v = Rc::new(RefCell::new(v));
        self.vertices.push(Rc::clone(&v));
        v
    }
    
    fn add_edge(&mut self, v1: &RcVertex<V>, v2: &RcVertex<V>) {
        v1.borrow_mut().neighbours.push(Rc::downgrade(&v2));
        v2.borrow_mut().neighbours.push(Rc::downgrade(&v1));
    }
    
    fn dft(start: RcVertex<V>, f: impl Fn(&V)) {
        let mut s = vec![];
        Self::dft_helper(start, &f, &mut s);
    }
    
    fn dft_helper(start: RcVertex<V>, f: &impl Fn(&V), s: &mut Vec<*const Vertex<V>>) {
        s.push(start.as_ptr());
        (f)(&start.borrow().value);
        for n in &start.borrow().neighbours {
            let n = n.upgrade().expect("Invalid neighbor");
            if s.iter().all(|&p| p != n.as_ptr()) {
                Self::dft_helper(n, f, s);
            }
        }
    }
}

fn main() {
    let mut g = Graph::new();
    
    let v1 = g.new_vertex(1);
    let v2 = g.new_vertex(2);
    let v3 = g.new_vertex(3);
    let v4 = g.new_vertex(4);
    let v5 = g.new_vertex(5);
    
    g.add_edge(&v1, &v2);
    g.add_edge(&v1, &v4);
    g.add_edge(&v1, &v5);
    g.add_edge(&v2, &v3);
    g.add_edge(&v3, &v4);
    g.add_edge(&v4, &v5);
    
    Graph::dft(v1, |v| println!("{}", v));
}
```

```rust
1
2
3
4
5
```



<br>

### 131. <font color="0c0a3e">Successive conditions</font>


> Execute f1 if condition c1 is true, or else f2 if condition c2 is true, or else f3 if condition c3 is true.
Don't evaluate a condition when a previous condition was true.


*连续条件判等*

```go
package main

import (
	"fmt"
	"strings"
)

func conditional(x string) {
	switch {
	case c1(x):
		f1()
	case c2(x):
		f2()
	case c3(x):
		f3()
	}
}

func main() {
	conditional("dog Snoopy")
	conditional("fruit Raspberry")
}

func f1() {
	fmt.Println("I'm a Human")
}

func f2() {
	fmt.Println("I'm a Dog")
}

func f3() {
	fmt.Println("I'm a Fruit")
}

var c1, c2, c3 = prefixCheck("human"), prefixCheck("dog"), prefixCheck("fruit")

func prefixCheck(prefix string) func(string) bool {
	return func(x string) bool {
		return strings.HasPrefix(x, prefix)
	}
}
```

```go
I'm a Dog
I'm a Fruit
```

---

```rust
if c1 { f1() } else if c2 { f2() } else if c3 { f3() }
```

or


```rust
match true {
    _ if c1 => f1(),
    _ if c2 => f2(),
    _ if c3 => f3(),
    _ => (),
}
```

<br>

### 132. <font color="0c0a3e">Measure duration of procedure execution</font>


> Run procedure f, and return the duration of the execution of f.


*度量程序执行时间*

```go
package main

import (
	"fmt"
	"regexp"
	"strings"
	"time"
)

func clock(f func()) time.Duration {
	t := time.Now()
	f()
	return time.Since(t)
}

func f() {
	re := regexp.MustCompilePOSIX("|A+{300}")
	re.FindAllString(strings.Repeat("A", 299), -1)
}

func main() {
	d := clock(f)

	// The result is always zero in the playground, which has a fixed clock!
	// Try it on your workstation instead.
	fmt.Println(d)
}

```

`0s`

---

```rust
use std::time::Instant;
let start = Instant::now();
f();
let duration = start.elapsed();
```

<br>


### 133. <font color="0c0a3e">Case-insensitive string contains</font>

> Set boolean ok to true if string word is contained in string s as a substring, even if the case doesn't match, or to false otherwise.


*不区分大小写的字符串包含*

```go
package main

import (
	"fmt"
	"strings"
)

// Package _strings has no case-insensitive version of _Contains, so
// we have to make our own.
func containsCaseInsensitive(s, word string) bool {
	lowerS, lowerWord := strings.ToLower(s), strings.ToLower(word)
	ok := strings.Contains(lowerS, lowerWord)
	return ok
}

func main() {
	s := "Let's dance the macarena"

	word := "Dance"
	ok := containsCaseInsensitive(s, word)
	fmt.Println(ok)

	word = "dance"
	ok = containsCaseInsensitive(s, word)
	fmt.Println(ok)

	word = "Duck"
	ok = containsCaseInsensitive(s, word)
	fmt.Println(ok)
}

```

```go
true
true
false
```

---

```rust
extern crate regex;
use regex::Regex;

fn main() {
    let s = "Let's dance the macarena";

    {
        let word = "Dance";
        let re = Regex::new(&format!("(?i){}", regex::escape(word))).unwrap();
        let ok = re.is_match(&s);

        println!("{}", ok);
    }
    
    {
        let word = "dance";
        let re = Regex::new(&format!("(?i){}", regex::escape(word))).unwrap();
        let ok = re.is_match(&s);

        println!("{}", ok);
    }
    
    {
        let word = "Duck";
        let re = Regex::new(&format!("(?i){}", regex::escape(word))).unwrap();
        let ok = re.is_match(&s);

        println!("{}", ok);
    }
}

```


```rust
true
true
false
```


or



```rust
use regex::RegexBuilder;

fn main() {
    let s = "FooBar";
    let word = "foo";
    
    let re = RegexBuilder::new(&regex::escape(word))
        .case_insensitive(true)
        .build()
        .unwrap();

    let ok = re.is_match(s);
    
    println!("{:?}", ok);
}
```

`true`

or

```rust
fn main() {
    let s = "Let's dance the macarena";

    {
        let word = "Dance";
        let ok = s.to_ascii_lowercase().contains(&word.to_ascii_lowercase());
        println!("{}", ok);
    }

    {
        let word = "dance";
        let ok = s.to_ascii_lowercase().contains(&word.to_ascii_lowercase());
        println!("{}", ok);
    }

    {
        let word = "Duck";
        let ok = s.to_ascii_lowercase().contains(&word.to_ascii_lowercase());
        println!("{}", ok);
    }
}
```

```rust
true
true
false
```

<br>

### 134. <font color="0c0a3e">Create a new list</font>

*创建一个新list*

```go
package main

import (
	"fmt"
)

func main() {
	var a, b, c T = "This", "is", "wonderful"

	items := []T{a, b, c}

	fmt.Println(items)
}

type T string

```


`[This is wonderful]`

---

```rust
fn main() {
    let (a, b, c) = (11, 22, 33);
    
    let items = vec![a, b, c];
    
    println!("{:?}", items);
}
```

`[11, 22, 33]`

<br>

### 135. <font color="0c0a3e">Remove item from list, by its value</font>

> Remove at most 1 item from list items, having value x.
This will alter the original list or return a new list, depending on which is more idiomatic.
If there are several occurrences of x in items, remove only one of them. If x is absent, keep items unchanged.


*移除列表中的值*

```go
package main

import (
	"fmt"
)

func main() {
	items := []string{"a", "b", "c", "d", "e", "f"}
	fmt.Println(items)

	x := "c"
	for i, y := range items {
		if y == x {
			items = append(items[:i], items[i+1:]...)
			break
		}
	}
	fmt.Println(items)
}

```


```go
[a b c d e f]
[a b d e f]
```


or

```go
for i, y := range items {
	if y == x {
		copy(items[i:], items[i+1:])
		items[len(items)-1] = nil
		items = items[:len(items)-1]
		break
	}
}
```


---

```rust
if let Some(i) = items.first(&x) {
    items.remove(i);
}
```

<br>

### 136. <font color="0c0a3e"> Remove all occurrences of a value from a list</font>

> Remove all occurrences of value x from list items.
This will alter the original list or return a new list, depending on which is more idiomatic.

*从列表中删除所有出现的值*

```go
package main

import (
	"fmt"
)

func main() {
	items := []T{"b", "a", "b", "a", "r"}
	fmt.Println(items)

	var x T = "b"
	items2 := make([]T, 0, len(items))
	for _, v := range items {
		if v != x {
			items2 = append(items2, v)
		}
	}

	fmt.Println(items2)
}

type T string

```

```go
[b a b a r]
[a a r]
```

or

```go
package main

import (
	"fmt"
)

func main() {
	items := []T{"b", "a", "b", "a", "r"}
	fmt.Println(items)

	x := T("b")
	j := 0
	for i, v := range items {
		if v != x {
			items[j] = items[i]
			j++
		}
	}
	items = items[:j]

	fmt.Println(items)
}

type T string

```

```go
[b a b a r]
[a a r]
```

or

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	var items []*image
	{
		red := newUniform(rgb{0xFF, 0, 0})
		white := newUniform(rgb{0xFF, 0xFF, 0xFF})
		items = []*image{red, white, red} // Like the flag of Austria
		fmt.Println("items =", items)

		x := red
		j := 0
		for i, v := range items {
			if v != x {
				items[j] = items[i]
				j++
			}
		}
		for k := j; k < len(items); k++ {
			items[k] = nil
		}
		items = items[:j]
	}

	// At this point, red can be garbage collected

	printAllocInfo()

	fmt.Println("items =", items) // Not the original flag anymore...
	fmt.Println("items undelying =", items[:3])
}

type image [1024][1024]rgb
type rgb [3]byte

func newUniform(color rgb) *image {
	im := new(image)
	for x := range im {
		for y := range im[x] {
			im[x][y] = color
		}
	}
	return im
}

func printAllocInfo() {
	var stats runtime.MemStats
	runtime.GC()
	runtime.ReadMemStats(&stats)
	fmt.Println("Bytes allocated (total):", stats.TotalAlloc)
	fmt.Println("Bytes still allocated:  ", stats.Alloc)
}

```

```go
items = [0xc000180000 0xc000480000 0xc000180000]
Bytes allocated (total): 6416688
Bytes still allocated:   3259024
items = [0xc000480000]
items undelying = [0xc000480000 <nil> <nil>]
```



---

```rust
fn main() {
    let x = 1;
    let mut items = vec![1, 2, 3, 1, 2, 3];
    
    items = items.into_iter().filter(|&item| item != x).collect();
    
    println!("{:?}", items);
}

```

`[2, 3, 2, 3]`

<br>


### 137. <font color="0c0a3e">Check if string contains only digits</font>


> Set boolean b to true if string s contains only characters in range '0'..'9', false otherwise.


*检查字符串是否只包含数字*

```go
package main

import (
	"fmt"
)

func main() {
	for _, s := range []string{
		"123",
		"",
		"abc123def",
		"abc",
		"123.456",
		"123 456",
	} {
		b := true
		for _, c := range s {
			if c < '0' || c > '9' {
				b = false
				break
			}
		}
		fmt.Println(s, "=>", b)
	}
}

```


```go
123 => true
 => true
abc123def => false
abc => false
123.456 => false
123 456 => false
```


or

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	for _, s := range []string{
		"123",
		"",
		"abc123def",
		"abc",
		"123.456",
		"123 456",
	} {
		isNotDigit := func(c rune) bool { return c < '0' || c > '9' }
		b := strings.IndexFunc(s, isNotDigit) == -1
		fmt.Println(s, "=>", b)
	}
}

```


```go
123 => true
 => true
abc123def => false
abc => false
123.456 => false
123 456 => false
```

---

```rust
fn main() {
    let s = "1023";
    let chars_are_numeric: Vec<bool> = s.chars().map(|c|c.is_numeric()).collect();
    let b = !chars_are_numeric.contains(&false);
    println!("{}", b);
}
```

`true`

or

```rust
fn main() {
    let b = "0129".chars().all(char::is_numeric);
    println!("{}", b);
}
```

`true`

<br>

### 138. <font color="0c0a3e">Create temp file</font>


> Create a new temporary file on filesystem.

*创建一个新的临时文件*

```go
package main

import (
	"io/ioutil"
	"log"
	"os"
)

func main() {
	content := []byte("Big bag of misc data")

	log.Println("Opening new temp file")
	tmpfile, err := ioutil.TempFile("", "example")
	if err != nil {
		log.Fatal(err)
	}
	tmpfilename := tmpfile.Name()
	defer os.Remove(tmpfilename) // clean up
	log.Println("Opened new file", tmpfilename)

	log.Println("Writing [[", string(content), "]]")
	if _, err := tmpfile.Write(content); err != nil {
		log.Fatal(err)
	}
	if err := tmpfile.Close(); err != nil {
		log.Fatal(err)
	}
	log.Println("Closed", tmpfilename)

	log.Println("Opening", tmpfilename)
	buffer, err := ioutil.ReadFile(tmpfilename)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("Read[[", string(buffer), "]]")
}

```

```go
2009/11/10 23:00:00 Opening new temp file
2009/11/10 23:00:00 Opened new file /tmp/example067319278
2009/11/10 23:00:00 Writing [[ Big bag of misc data ]]
2009/11/10 23:00:00 Closed /tmp/example067319278
2009/11/10 23:00:00 Opening /tmp/example067319278
2009/11/10 23:00:00 Read[[ Big bag of misc data ]]
```

---

```rust
use tempdir::TempDir;
use std::fs::File;
let temp_dir = TempDir::new("prefix")?;
let temp_file = File::open(temp_dir.path().join("file_name"))?;
```

<br>

### 139. <font color="0c0a3e">Create temp directory</font>

> Create a new temporary folder on filesystem, for writing.


*创建一个临时目录*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"os"
	"path/filepath"
)

func main() {
	content := []byte("temporary file's content")
	dir, err := ioutil.TempDir("", "")
	if err != nil {
		log.Fatal(err)
	}

	defer os.RemoveAll(dir) // clean up

	inspect(dir)

	tmpfn := filepath.Join(dir, "tmpfile")
	err = ioutil.WriteFile(tmpfn, content, 0666)
	if err != nil {
		log.Fatal(err)
	}

	inspect(dir)
}

func inspect(dirpath string) {
	files, err := ioutil.ReadDir(dirpath)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(dirpath, "contains", len(files), "files")
}

```

```go
/tmp/067319278 contains 0 files
/tmp/067319278 contains 1 files
```

---

```rust
extern crate tempdir;
use tempdir::TempDir;
let tmp = TempDir::new("prefix")?;
```

<br>

### 140. <font color="0c0a3e">Delete map entry</font>




*从map中删除某个key*

```go
package main

import (
	"fmt"
)

func main() {
	m := map[string]int{
		"uno":  1,
		"dos":  2,
		"tres": 3,
	}

	delete(m, "dos")
	delete(m, "cinco")

	fmt.Println(m)
}

```

`map[tres:3 uno:1]`

---

```rust
fn main() {
    use std::collections::HashMap;

    let mut m = HashMap::new();
    m.insert(5, "a");
    m.insert(17, "b");
    println!("{:?}", m);

    m.remove(&5);
    println!("{:?}", m);
}
```

```rust
{17: "b", 5: "a"}
{17: "b"}
```

<br>

