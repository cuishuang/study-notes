---
title: 'Rust vs Go:常用语法对比(2)'
date: 2021-09-03 01:35:16
tags: Rust
---



<br>

### 21. <font color="70d6ff">Swap values</font>

*交换变量a和b的值*

```go
a, b = b, a
```

```go
package main

import "fmt"

func main() {
	a := 3
	b := 10
	a, b = b, a
	fmt.Println(a)
	fmt.Println(b)
}
```

```go
10
3
```

---

```rust
fn main() {
    let a = 3;
    let b = 10;

    let (a, b) = (b, a);

    println!("a: {a}, b: {b}", a=a, b=b);
}
```

输出

`a: 10, b: 3`

or

```rust
fn main() {
    let (a, b) = (12, 42);
    
    println!("a = {}, b = {}", a, b);
    
    let (a, b) = (b, a);
    
    println!("a = {}, b = {}", a, b);
}
```

输出

```rust
a = 12, b = 42
a = 42, b = 12
```


<br>

### 22. <font color="ff70a6">Convert string to integer</font>



*将字符串转换为整型*



```go
import "strconv"
i, err  := strconv.Atoi(s) 
```

```go
package main

import (
	"fmt"
	"reflect"
	"strconv"
)

func main() {
	// create a string
	s := "123"
	fmt.Println(s)
	fmt.Println("type:", reflect.TypeOf(s))

	// convert string to int
	i, err := strconv.Atoi(s)
	if err != nil {
		panic(err)
	}
	fmt.Println(i)
	fmt.Println("type:", reflect.TypeOf(i))
}
```


```go
123
type: string
123
type: int
```

or

```go
import "strconv"
i, err := strconv.ParseInt(s, 10, 0)
```

```go
package main

import (
	"fmt"
	"reflect"
	"strconv"
)

func main() {
	s := "123"
	fmt.Println("s is", reflect.TypeOf(s), s)

	i, err := strconv.ParseInt(s, 10, 0)
	if err != nil {
		panic(err)
	}

	fmt.Println("i is", reflect.TypeOf(i), i)
}

```

```go
s is string 123
i is int64 123
```


---

```rust
fn main() {
    // This prints 123
    let mut s = "123";
    let mut i = s.parse::<i32>().unwrap();
    println!("{:?}", i);

    // This panics
    s = "12u3";
    i = s.parse::<i32>().unwrap();
    println!("{:?}", i);
}
```

or

```rust
fn main() {
    let mut s = "123";
    let mut i: i32 = s.parse().unwrap_or(0);
    println!("{:?}", i);

    s = "12u3";
    i = s.parse().unwrap_or(0);
    println!("{:?}", i);
}
```

输出

```rust
123
0
```

or


```rust
fn main() {
    let mut s = "123";
    let mut i = match s.parse::<i32>() {
        Ok(i) => i,
        Err(_e) => -1,
    };
    println!("{:?}", i);

    s = "12u3";
    i = match s.parse::<i32>() {
        Ok(i) => i,
        Err(_e) => -1,
    };
    println!("{:?}", i);
}
```

输出

```rust
123
-1
```


<br>

### 23. <font color="ff9770">Convert real number to string with 2 decimal places</font>

> Given a real number x, create its string representation s with 2 decimal digits following the dot.

*给定一个实数，小数点后保留两位小数*

```go
package main

import "fmt"

func main() {
	x := 3.14159
	s := fmt.Sprintf("%.2f", x)
	fmt.Println(s)
}

```

输出

`3.14`

---

```rust
fn main() {
    let x = 42.1337;
    let s = format!("{:.2}", x);
    
    println!("{}", s);
}
```

输出

`42.13`


<br>

### 24. <font color="ffd670">Assign to string the japanese word ネコ</font>

> Declare a new string s and initialize it with the literal value "ネコ" (which means "cat" in japanese)



*声明一个新的字符串s，并用文字值“ネコ”初始化它(在日语中是“cat”的意思)*


```go
package main

import "fmt"

func main() {
	s := "ネコ"
	fmt.Println(s)
}

```

---


```rust
fn main() {
    let s = "ネコ";
    println!("{}", s);
}
```



<br>

### 25. <font color="e9ff70">Send a value to another thread</font>


> Share the string value "Alan" with an existing running process which will then display "Hello, Alan"

*将字符串值“Alan”与现有的正在运行的进程共享，该进程将显示“你好，Alan”*

```go
ch <- "Alan"
```

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan string)

	go func() {
		v := <-ch
		fmt.Printf("Hello, %v\n", v)
	}()

	ch <- "Alan"

	// Make sure the non-main goroutine had the chance to finish.
	time.Sleep(time.Second)
}
```


`Hello, Alan`


*The receiver goroutine blocks reading the string channel ch. 
The current goroutine sends the value to ch. 
A goroutine is like a thread, but more lightweight.*



---

```rust
use std::thread;
use std::sync::mpsc::channel;

fn main() {
    let (send, recv) = channel();

    let handle = thread::spawn(move || loop {
        let msg = recv.recv().unwrap();
        println!("Hello, {:?}", msg);
    });

    send.send("Alan").unwrap();
    
    handle.join().unwrap();
}

```


输出 `Hello, "Alan"`

<br>

### 26. <font color="386641">Create a 2-dimensional array</font>

>Declare and initialize a matrix x having m rows and n columns, containing real numbers.

**创建一个二维数组**

*声明并初始化一个有m行n列的矩阵x，包含实数。*



```go
const m, n = 3, 4
var x [m][n]float64
```

```go
package main

import "fmt"

func main() {
	const m, n = 3, 4
	var x [m][n]float64

	x[1][2] = 8
	fmt.Println(x)
}
```


`[[0 0 0 0] [0 0 8 0] [0 0 0 0]]`


or

```go
package main

import "fmt"

func main() {
	x := make2D(2, 3)

	x[1][1] = 8
	fmt.Println(x)
}

func make2D(m, n int) [][]float64 {
	buf := make([]float64, m*n)

	x := make([][]float64, m)
	for i := range x {
		x[i] = buf[:n:n]
		buf = buf[n:]
	}
	return x
}

```

`[[0 0 0] [0 8 0]]`

---


```rust
fn main() {
    const M: usize = 4;
    const N: usize = 6;

    let x = vec![vec![0.0f64; N]; M];
    
    println!("{:#?}", x);
}

```
输出

```rust
[
    [
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
    ],
    [
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
    ],
    [
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
    ],
    [
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
    ],
]
```

```rust
fn main() {
  const M: usize = 3;
  const N: usize = 4;

  let mut x = [[0.0; N] ; M];

  x[1][3] = 5.0;
  println!("{:#?}", x);
}
```

输出

```rust
[
    [
        0.0,
        0.0,
        0.0,
        0.0,
    ],
    [
        0.0,
        0.0,
        0.0,
        5.0,
    ],
    [
        0.0,
        0.0,
        0.0,
        0.0,
    ],
]
```

<br>

### 27. <font color="6a994e">Create a 3-dimensional array</font>

> Declare and initialize a 3D array x, having dimensions boundaries m, n, p, and containing real numbers.

**创建一个三维数组**

*声明并初始化一个三维数组x，它有m，n，p维边界，并且包含实数。*




```go
const m, n, p = 2, 2, 3
var x [m][n][p]float64
```

```go
package main

import "fmt"

func main() {
	const m, n, p = 2, 2, 3
	var x [m][n][p]float64

	x[1][0][2] = 9

	// Value of x
	fmt.Println(x)

	// Type of x
	fmt.Printf("%T", x)
}
```

```go
[[[0 0 0] [0 0 0]] [[0 0 9] [0 0 0]]]
[2][2][3]float64
```

or

```go
func make3D(m, n, p int) [][][]float64 {
	buf := make([]float64, m*n*p)

	x := make([][][]float64, m)
	for i := range x {
		x[i] = make([][]float64, n)
		for j := range x[i] {
			x[i][j] = buf[:p:p]
			buf = buf[p:]
		}
	}
	return x
}
```

```go
package main

import "fmt"

func main() {
	x := make3D(2, 2, 3)

	x[1][0][2] = 9
	fmt.Println(x)
}

func make3D(m, n, p int) [][][]float64 {
	buf := make([]float64, m*n*p)

	x := make([][][]float64, m)
	for i := range x {
		x[i] = make([][]float64, n)
		for j := range x[i] {
			x[i][j] = buf[:p:p]
			buf = buf[p:]
		}
	}
	return x
}
```

```go
[[[0 0 0] [0 0 0]] [[0 0 9] [0 0 0]]]
```

---


```rust
fn main() {
    let m = 4;
    let n = 6;
    let p = 2;

    let x = vec![vec![vec![0.0f64; p]; n]; m];
    
    println!("{:#?}", x);
}
```
输出

```rust

[
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
]
```


```rust
fn main() {
    const M: usize = 4;
    const N: usize = 6;
    const P: usize = 2;

    let x = [[[0.0f64; P]; N]; M];

    println!("{:#?}", x);
}
```

输出

```rust
[
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
    [
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
        [
            0.0,
            0.0,
        ],
    ],
]
```


<br>

### 28. <font color="a7c957">Sort by a property</font>

> Sort elements of array-like collection items in ascending order of x.p, where p is a field of the type Item of the objects in items.


*按x->p的升序对类似数组的集合项的元素进行排序，其中p是项中对象的项类型的字段。*



```go
package main

import "fmt"
import "sort"

type Item struct {
	label string
	p     int
	lang  string
}

type ItemPSorter []Item

func (s ItemPSorter) Len() int           { return len(s) }
func (s ItemPSorter) Less(i, j int) bool { return s[i].p < s[j].p }
func (s ItemPSorter) Swap(i, j int)      { s[i], s[j] = s[j], s[i] }

func sortItems(items []Item) {
	sorter := ItemPSorter(items)
	sort.Sort(sorter)
}

func main() {
	items := []Item{
		{"twelve", 12, "english"},
		{"six", 6, "english"},
		{"eleven", 11, "english"},
		{"zero", 0, "english"},
		{"two", 2, "english"},
	}
	fmt.Println("Unsorted: ", items)
	sortItems(items)
	fmt.Println("Sorted: ", items)
}

```

```go
Unsorted:  [{twelve 12 english} {six 6 english} {eleven 11 english} {zero 0 english} {two 2 english}]
Sorted:  [{zero 0 english} {two 2 english} {six 6 english} {eleven 11 english} {twelve 12 english}]
```


or

```go
package main

import "fmt"
import "sort"

type Item struct {
	label string
	p     int
	lang  string
}

func main() {
	items := []Item{
		{"twelve", 12, "english"},
		{"six", 6, "english"},
		{"eleven", 11, "english"},
		{"zero", 0, "english"},
		{"two", 2, "english"},
	}
	fmt.Println("Unsorted: ", items)

	less := func(i, j int) bool {
		return items[i].p < items[j].p
	}
	sort.Slice(items, less)

	fmt.Println("Sorted: ", items)
}

```

```go
Unsorted:  [{twelve 12 english} {six 6 english} {eleven 11 english} {zero 0 english} {two 2 english}]
Sorted:  [{zero 0 english} {two 2 english} {six 6 english} {eleven 11 english} {twelve 12 english}]
```

---

```rust
#[derive(Debug)]
struct Foo {
    p: i32,
}

fn main() {
    let mut items = vec![Foo { p: 3 }, Foo { p: 1 }, Foo { p: 2 }, Foo { p: 4 }];

    items.sort_by(|a, b| a.p.cmp(&b.p));

    println!("{:?}", items);
}

```

输出


```rust
[Foo { p: 1 }, Foo { p: 2 }, Foo { p: 3 }, Foo { p: 4 }]
```

or

```rust
#[derive(Debug)]
struct Foo {
    p: i32,
}

fn main() {
    let mut items = vec![Foo { p: 3 }, Foo { p: 1 }, Foo { p: 2 }, Foo { p: 4 }];

    items.sort_by_key(|x| x.p);

    println!("{:?}", items);
}
```

输出

```rust
[Foo { p: 1 }, Foo { p: 2 }, Foo { p: 3 }, Foo { p: 4 }]
```



<br>

### 29. <font color="f2e8cf">Remove item from list, by its index</font>

> Remove i-th item from list items.
This will alter the original list or return a new list, depending on which is more idiomatic.
Note that in most languages, the smallest valid value for i is 0.



*从列表项中删除第I项。
这将改变原来的列表或返回一个新的列表，这取决于哪个更习惯。
请注意，在大多数语言中，I的最小有效值是0。*



```go
package main

import (
	"fmt"
)

func main() {
	items := []string{"a", "b", "c", "d", "e", "f"}
	fmt.Println(items)

	i := 2
	items = append(items[:i], items[i+1:]...)
	fmt.Println(items)
}
```

```go
[a b c d e f]
[a b d e f]
```

or

```go
copy(items[i:], items[i+1:])
items[len(items)-1] = nil
items = items[:len(items)-1]
```

---

```rust
fn main() {
    let mut v = vec![1, 2, 3];
    assert_eq!(v.remove(1), 2);
    assert_eq!(v, [1, 3]);
    
}
```


<br>

### 30. <font color="bc4749">	Parallelize execution of 1000 independent tasks</font>

> Launch the concurrent execution of procedure f with parameter i from 1 to 1000.
Tasks are independent and f(i) doesn't return any value.
Tasks need not run all at the same time, so you may use a pool.

*用参数I从1到1000启动程序f的并发执行。
任务是独立的，f(i)不返回值。
任务不需要同时运行，所以可以使用pools*



```go
import "sync"
wg := sync.WaitGroup{}
wg.Add(1000)
for i := 1; i <= 1000; i++ {
	go func(j int) {
          f(j)
          wg.Done()
        }(i)
}
wg.Wait()
```

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func f(i int) {
	d := rand.Int() % 10000
	time.Sleep(time.Duration(d))
	fmt.Printf("Hello %v\n", i)
}

func main() {
	for i := 1; i <= 1000; i++ {
		go f(i)
	}

	time.Sleep(4 * time.Second)
}

```

---

```rust
use std::thread;

fn main() {
    let threads: Vec<_> = (0..1000).map(|i| thread::spawn(move || f(i))).collect();

    for thread in threads {
        thread.join();
    }
}

fn f(i: i32) {
    println!("{}", i);
}

```

or

```rust
extern crate rayon;

use rayon::prelude::*;

fn main() {
    (0..1000).into_par_iter().for_each(f);
}

fn f(i: i32) {
    println!("{}", i);
}
```

<br>

### 31. <font color="001524">Recursive factorial (simple)</font>

> Create recursive function f which returns the factorial of non-negative integer i, calculated from f(i-1)

*创建递归函数f，该函数返回从f(i-1)计算的非负整数I的阶乘*


```go
func f(i int) int {
  if i == 0 {
    return 1
  }
  return i * f(i-1)
}
```

```go
package main

import (
	"fmt"
)

func f(i int) int {
	if i == 0 {
		return 1
	}
	return i * f(i-1)
}

func main() {
	for i := 0; i <= 10; i++ {
		fmt.Printf("f(%d) = %d\n", i, f(i))
	}
}

```

输出

```go
f(0) = 1
f(1) = 1
f(2) = 2
f(3) = 6
f(4) = 24
f(5) = 120
f(6) = 720
f(7) = 5040
f(8) = 40320
f(9) = 362880
f(10) = 3628800
```

---


```rust
fn f(n: u32) -> u32 {
    if n < 2 {
        1
    } else {
        n * f(n - 1)
    }
}

fn main() {
    println!("{}", f(4 as u32));
}
```

输出

`24`


or


```rust
fn factorial(num: u64) -> u64 {
    match num {
        0 | 1=> 1,
        _ => factorial(num - 1) * num,
    }
}

fn main (){
    println!("{}", factorial(0));
    println!("{}", factorial(1));
    println!("{}", factorial(2));
    println!("{}", factorial(3));
    println!("{}", factorial(4));
    println!("{}", factorial(5));
}

```

输出

```rust
1
1
2
6
24
120
```


<br>

### 32. <font color="15616d">Integer exponentiation by squaring</font>

> Create function exp which calculates (fast) the value x power n.
x and n are non-negative integers.

*创建函数exp，计算(快速)x次方n的值。
x和n是非负整数。*



```go
package main

import "fmt"

func exp(x, n int) int {
	switch {
	case n == 0:
		return 1
	case n == 1:
		return x
	case n%2 == 0:
		return exp(x*x, n/2)
	default:
		return x * exp(x*x, (n-1)/2)
	}
}

func main() {
	fmt.Println(exp(3, 5))
}
```

输出

`243`

---

```rust
fn exp(x: u64, n: u64) -> u64 {
    match n {
        0 => 1,
        1 => x,
        i if i % 2 == 0 => exp(x * x, n / 2),
        _ => x * exp(x * x, (n - 1) / 2),
    }
}

fn main() {
    let x = 16;
    let n = 4;
    
    println!("{}", exp(x, n));
}

```

输出

`65536`


<br>

### 33. <font color="ffecd1">Atomically read and update variable</font>


> Assign variable x the new value f(x), making sure that no other thread may modify x between the read and the write.

*为变量x分配新值f(x)，确保在读和写之间没有其他线程可以修改x。*


```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var lock sync.RWMutex
	x := 3

	lock.Lock()
	x = f(x)
	lock.Unlock()

	fmt.Println(x)
}

func f(i int) int {
	return 2 * i
}

```

`6`

---

```rust
use std::sync::Mutex;

fn f(x: i32) -> i32 {
    x + 1
}

fn main() {
    let x = Mutex::new(0);
    let mut x = x.lock().unwrap();
    *x = f(*x);
    
    println!("{:?}", *x);
}

```

输出

`1`



<br>

### 34. <font color="ff7d00">Create a set of objects</font>


> Declare and initialize a set x containing objects of type T.

*声明并初始化一个包含t类型对象的集合x。*




```go
x := make(map[T]bool)
```

```go
package main

import "fmt"

type T string

func main() {
	// declare a Set (implemented as a map)
	x := make(map[T]bool)

	// add some elements
	x["A"] = true
	x["B"] = true
	x["B"] = true
	x["C"] = true
	x["D"] = true

	// remove an element
	delete(x, "C")

	for t, _ := range x {
		fmt.Printf("x contains element %v \n", t)
	}
}
```

```go
x contains element D 
x contains element A 
x contains element B 
```

or


```go
x := make(map[T]struct{})
```

```go
package main

import "fmt"

type T string

func main() {
	// declare a Set (implemented as a map)
	x := make(map[T]struct{})

	// add some elements
	x["A"] = struct{}{}
	x["B"] = struct{}{}
	x["B"] = struct{}{}
	x["C"] = struct{}{}
	x["D"] = struct{}{}

	// remove an element
	delete(x, "C")

	for t, _ := range x {
		fmt.Printf("x contains element %v \n", t)
	}
}
```

```go
x contains element B 
x contains element D 
x contains element A 
```


---

```rust
use std::collections::HashSet;

fn main() {
    let mut m = HashSet::new();
    m.insert("a");
    m.insert("b");

    println!("{:?}", m);
}
```

输出

```rust
{"a", "b"}
```


<br>

### 35. <font color="78290f">[First-class function : compose](https://programming-idioms.org/idiom/35/first-class-function-compose)</font>


> Implement a function compose (A -> C) with parameters f (A -> B) and g (B -> C), which returns composition function g ∘ f

*用参数f (A -> B)和g (B -> C)实现一个函数compose (A -> C)，返回composition函数g ∘ f*


```go
package main

import "fmt"
import "strconv"

func compose(f func(A) B, g func(B) C) func(A) C {
	return func(x A) C {
		return g(f(x))
	}
}

func main() {
	squareFromStr := compose(str2int, square)
	fmt.Println(squareFromStr("12"))
}

type A string
type B int
type C int

func str2int(a A) B {
	b, _ := strconv.ParseInt(string(a), 10, 32)
	return B(b)
}

func square(b B) C {
	return C(b * b)
}

```

`144`


---

```rust
fn compose<'a, A, B, C, G, F>(f: F, g: G) -> Box<Fn(A) -> C + 'a>
	where F: 'a + Fn(A) -> B, G: 'a + Fn(B) -> C
{
	Box::new(move |x| g(f(x)))
}
```

or

```rust
fn compose<A, B, C>(f: impl Fn(A) -> B, g: impl Fn(B) -> C) -> impl Fn(A) -> C {
	move |x| g(f(x))
}

fn main() {
    let f = |x: u32| (x * 2) as i32;
    let g = |x: i32| (x + 1) as f32;
    let c = compose(f, g);
    
    println!("{}", c(2));
}
```
输出

`5`


<br>

### 36. <font color="ff9f1c">First-class function : generic composition</font>


> Implement a function compose which returns composition function g ∘ f for any functions f and g having exactly 1 parameter.

*实现一个函数组合，该函数组合为任何恰好有1个参数的函数f和g返回组合函数g ∘ f。*


```go
package main

import "fmt"

func composeIntFuncs(f func(int) int, g func(int) int) func(int) int {
	return func(x int) int {
		return g(f(x))
	}
}

func main() {
	double := func(x int) int {
		return 2 * x
	}
	addTwo := func(x int) int {
		return x + 2
	}
	h := composeIntFuncs(double, addTwo)

	for i := 0; i < 10; i++ {
		fmt.Println(i, h(i), addTwo(double(i)))
	}
}
```

```go
0 2 2
1 4 4
2 6 6
3 8 8
4 10 10
5 12 12
6 14 14
7 16 16
8 18 18
9 20 20
```


---

```rust
fn compose<'a, A, B, C, G, F>(f: F, g: G) -> Box<Fn(A) -> C + 'a>
	where F: 'a + Fn(A) -> B, G: 'a + Fn(B) -> C
{
	Box::new(move |x| g(f(x)))
}
```

or

```rust
fn compose<A, B, C>(f: impl Fn(A) -> B, g: impl Fn(B) -> C) -> impl Fn(A) -> C {
	move |x| g(f(x))
}

fn main() {
    let f = |x: u32| (x * 2) as i32;
    let g = |x: i32| (x + 1) as f32;
    let c = compose(f, g);
    
    println!("{}", c(2));
}
```

输出

`5`


<br>

### 37. <font color="ffbf69">Currying</font>

> Transform a function that takes multiple arguments into a function for which some of the arguments are preset.


*将一个接受多个参数的函数转换为一个预设了某些参数的函数。*


```go
package main

import (
	"fmt"
	"time"
)

type Company string

type Employee struct {
	FirstName string
	LastName  string
}

func (e *Employee) String() string {
	return "<" + e.FirstName + " " + e.LastName + ">"
}

type Payroll struct {
	Company   Company
	Boss      *Employee
	Employee  *Employee
	StartDate time.Time
	EndDate   time.Time
	Amount    int
}

// Creates a blank payroll for a specific employee with specific boss in specific company
type PayFactory func(Company, *Employee, *Employee) Payroll

// Creates a blank payroll for a specific employee
type CustomPayFactory func(*Employee) Payroll

func CurryPayFactory(pf PayFactory, company Company, boss *Employee) CustomPayFactory {
	return func(e *Employee) Payroll {
		return pf(company, boss, e)
	}
}

func NewPay(company Company, boss *Employee, employee *Employee) Payroll {
	return Payroll{
		Company:  company,
		Boss:     boss,
		Employee: employee,
	}
}

func main() {
	me := Employee{"Jack", "Power"}

	// I happen to be head of the HR department of Richissim Inc.
	var myLittlePayFactory CustomPayFactory = CurryPayFactory(NewPay, "Richissim", &me)

	fmt.Println(myLittlePayFactory(&Employee{"Jean", "Dupont"}))
	fmt.Println(myLittlePayFactory(&Employee{"Antoine", "Pol"}))
}

```


```go
{Richissim <Jack Power> <Jean Dupont> 0001-01-01 00:00:00 +0000 UTC 0001-01-01 00:00:00 +0000 UTC 0}
{Richissim <Jack Power> <Antoine Pol> 0001-01-01 00:00:00 +0000 UTC 0001-01-01 00:00:00 +0000 UTC 0}
```


---

```rust
fn add(a: u32, b: u32) -> u32 {
    a + b
}

fn main() {
    let add5 = move |x| add(5, x);

    let y = add5(12);
    println!("{}", y);
}
```

输出

`17`

<br>

### 38. <font color="cbf3f0">Extract a substring</font>

> Find substring t consisting in characters i (included) to j (excluded) of string s.
Character indices start at 0 unless specified otherwise.
Make sure that multibyte characters are properly handled.

*查找由字符串s的字符I(包括)到j(不包括)组成的子字符串t。
除非另有说明，字符索引从0开始。
确保正确处理多字节字符。*



```go
package main

import "fmt"

func main() {
	s := "hello, utf-8 문자들"
	i, j := 7, 15
	
	t := string([]rune(s)[i:j])
	
	fmt.Println(t)
}
```

`utf-8 문자`

---

```rust
extern crate unicode_segmentation;
use unicode_segmentation::UnicodeSegmentation;

fn main() {
    let s = "Lorem Ipsüm Dolor";
    let (i, j) = (6, 11);
    let t = s.graphemes(true).skip(i).take(j - i).collect::<String>();
    println!("{}", t);
}
```

输出

`Ipsüm`

or

```rust
use substring::Substring;
let t = s.substring(i, j);

```

<br>

### 39. <font color="2ec4b6">Check if string contains a word</font>

> Set boolean ok to true if string word is contained in string s as a substring, or to false otherwise.

*如果字符串单词作为子字符串包含在字符串s中，则将布尔ok设置为true，否则设置为false。*


```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "Let's dance the macarena"

	word := "dance"
	ok := strings.Contains(s, word)
	fmt.Println(ok)

	word = "car"
	ok = strings.Contains(s, word)
	fmt.Println(ok)

	word = "duck"
	ok = strings.Contains(s, word)
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
fn main() {
    let s = "Let's dance the macarena";

    let word = "dance";
    let ok = s.contains(word);
    println!("{}", ok);

    let word = "car";
    let ok = s.contains(word);
    println!("{}", ok);

    let word = "duck";
    let ok = s.contains(word);
    println!("{}", ok);
}
```

输出

```rust
true
true
false
```

<br>
