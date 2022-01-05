---
title: 'Rust vs Go:常用语法对比(13)'
date: 2021-09-14 23:43:58
tags: Rust
---


<br>

### 241. <font color="0c0a3e">Yield priority to other threads</font>

> Explicitly decrease the priority of the current process, so that other execution threads have a better chance to execute now. Then resume normal execution and call function **busywork**.



*将优先权让给其他线程*




```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	go fmt.Println("aaa")
	go fmt.Println("bbb")
	go fmt.Println("ccc")
	go fmt.Println("ddd")
	go fmt.Println("eee")

	runtime.Gosched()
	busywork()

	time.Sleep(100 * time.Millisecond)
}

func busywork() {
	fmt.Println("main")
}

```

**After Gosched, the execution of the current goroutine resumes automatically.**


```go
aaa
eee
ccc
bbb
ddd
main
```


---

```rust
::std::thread::yield_now();
busywork();
```

<br>


### 242. <font color="0c0a3e">Iterate over a set</font>

> Call a function f on each element e of a set x.



*迭代一个集合*

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

	for e := range x {
		f(e)
	}
}

func f(e T) {
	fmt.Printf("contains element %v \n", e)
}

```

```go
contains element A 
contains element B 
contains element D 
```

---


```rust
use std::collections::HashSet;

fn main() {
    let mut x = HashSet::new();
    x.insert("a");
    x.insert("b");

    for item in &x {
        f(item);
    }
}

fn f(s: &&str) {
    println!("Element {}", s);
}
```

**x is a HashSet**

```rust
Element a
Element b
```

<br>

### 243. <font color="0c0a3e">Print list</font>

> Print the contents of list a on the standard output.

*打印 list*

```go
package main

import (
	"fmt"
)

func main() {
	{
		a := []int{11, 22, 33}
		fmt.Println(a)
	}

	{
		a := []string{"aa", "bb"}
		fmt.Println(a)
	}

	{
		type Person struct {
			First string
			Last  string
		}
		x := Person{
			First: "Jane",
			Last:  "Doe",
		}
		y := Person{
			First: "John",
			Last:  "Doe",
		}
		a := []Person{x, y}
		fmt.Println(a)
	}

	{
		x, y := 11, 22
		a := []*int{&x, &y}
		fmt.Println(a)
	}
}

```

```go
[11 22 33]
[aa bb]
[{Jane Doe} {John Doe}]
[0xc000018080 0xc000018088]
```

---

```rust
fn main() {
    let a = [11, 22, 33];

    println!("{:?}", a);
}
```

`[11, 22, 33]`


<br>

### 244. <font color="0c0a3e">Print map</font>

> Print the contents of map m to the standard output: keys and values.

*打印 map*

```go
package main

import (
	"fmt"
)

func main() {
	{
		m := map[string]int{
			"eleven":     11,
			"twenty-two": 22,
		}
		fmt.Println(m)
	}

	{
		x, y := 7, 8
		m := map[string]*int{
			"seven": &x,
			"eight": &y,
		}
		fmt.Println(m)
	}
}

```

```go
map[eleven:11 twenty-two:22]
map[eight:0xc000100040 seven:0xc000100028]
```

---

```rust
use std::collections::HashMap;

fn main() {
    let mut m = HashMap::new();
    m.insert("Áron".to_string(), 23);
    m.insert("Béla".to_string(), 35);
    println!("{:?}", m);
}

```

`{"Béla": 35, "Áron": 23}`

<br>

### 245. <font color="0c0a3e">Print value of custom type</font>


> Print the value of object x having custom type T, for log or debug.


*打印自定义类型的值*

```go
package main

import (
	"fmt"
)

// T represents a tank. It doesn't implement fmt.Stringer.
type T struct {
	name      string
	weight    int
	firePower int
}

// Person implement fmt.Stringer.
type Person struct {
	FirstName   string
	LastName    string
	YearOfBirth int
}

func (p Person) String() string {
	return fmt.Sprintf("%s %s, born %d", p.FirstName, p.LastName, p.YearOfBirth)
}

func main() {
	{
		x := T{
			name:      "myTank",
			weight:    100,
			firePower: 90,
		}

		fmt.Println(x)
	}
	{
		x := Person{
			FirstName:   "John",
			LastName:    "Doe",
			YearOfBirth: 1958,
		}

		fmt.Println(x)
	}
}

```

*Will be more relevant if T implements fmt.Stringer*

```go
{myTank 100 90}
John Doe, born 1958
```

---

```rust
#[derive(Debug)]

// T represents a tank
struct T<'a> {
    name: &'a str,
    weight: &'a i32,
    fire_power: &'a i32,
}

fn main() {
    let x = T {
        name: "mytank",
        weight: &100,
        fire_power: &90,
    };

    println!("{:?}", x);
}

```

*Implement fmt::Debug or fmt::Display for T*

`T { name: "mytank", weight: 100, fire_power: 90 }`

<br>

### 246. <font color="0c0a3e">Count distinct elements</font>

> Set c to the number of distinct elements in list items.


*计算不同的元素的数量*

```go
package main

import (
	"fmt"
)

func main() {
	type T string
	items := []T{"a", "b", "b", "aaa", "c", "a", "d"}
	fmt.Println("items has", len(items), "elements")

	distinct := make(map[T]bool)
	for _, v := range items {
		distinct[v] = true
	}
	c := len(distinct)

	fmt.Println("items has", c, "distinct elements")
}
```

```go
items has 7 elements
items has 5 distinct elements
```

---

```rust
use itertools::Itertools;

fn main() {
    let items = vec!["víz", "árvíz", "víz", "víz", "ár", "árvíz"];
    let c = items.iter().unique().count();
    println!("{}", c);
}
```

`3`

<br>

### 247. <font color="0c0a3e">Filter list in-place</font>

> 	

Remove all the elements from list x that don't satisfy the predicate p, without allocating a new list.<br>Keep all the elements that do satisfy p.<br>For languages that don't have mutable lists, refer to idiom #57 instead.


*就地筛选列表*

```go
package main

import "fmt"

type T int

func main() {
	x := []T{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	p := func(t T) bool { return t%2 == 0 }

	j := 0
	for i, v := range x {
		if p(v) {
			x[j] = x[i]
			j++
		}
	}
	x = x[:j]

	fmt.Println(x)
}

```

`[2 4 6 8 10]`

or

```go
package main

import "fmt"

type T int

func main() {
	var x []*T
	for _, v := range []T{1, 2, 3, 4, 5, 6, 7, 8, 9, 10} {
		t := new(T)
		*t = v
		x = append(x, t)
	}
	p := func(t *T) bool { return *t%2 == 0 }

	j := 0
	for i, v := range x {
		if p(v) {
			x[j] = x[i]
			j++
		}
	}
	for k := j; k < len(x); k++ {
		x[k] = nil
	}
	x = x[:j]

	for _, pt := range x {
		fmt.Print(*pt, " ")
	}
}
```

`2 4 6 8 10`


---

```rust
fn p(t: i32) -> bool {
    t % 2 == 0
}

fn main() {
    let mut x = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let mut j = 0;
    for i in 0..x.len() {
        if p(x[i]) {
            x[j] = x[i];
            j += 1;
        }
    }
    x.truncate(j);
    println!("{:?}", x);
}

```

`[2, 4, 6, 8, 10]`

or

```rust
fn p(t: &i64) -> bool {
    t % 2 == 0
}

fn main() {
    let mut x: Vec<i64> = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    x.retain(p);

    println!("{:?}", x);
}
```

`[2, 4, 6, 8, 10]`



<br>

### 249. <font color="0c0a3e">Declare and assign multiple variables</font>

> Define variables a, b and c in a concise way.
Explain if they need to have the same type.


*声明并分配多个变量*

```go
package main

import (
	"fmt"
)

func main() {
	// a, b and c don't need to have the same type.

	a, b, c := 42, "hello", 5.0

	fmt.Println(a, b, c)
	fmt.Printf("%T %T %T \n", a, b, c)
}

```

```go
42 hello 5
int string float64 
```


---

```rust
fn main() {
    // a, b and c don't need to have the same type.

    let (a, b, c) = (42, "hello", 5.0);

    println!("{} {} {}", a, b, c);
}

```

`42 hello 5`



<br>

### 251. <font color="0c0a3e">Parse binary digits</font>

> Extract integer value i from its binary string representation s (in radix 2)
E.g. "1101" -> 13



*解析二进制数字*

```go
package main

import (
	"fmt"
	"reflect"
	"strconv"
)

func main() {
	s := "1101"
	fmt.Println("s is", reflect.TypeOf(s), s)

	i, err := strconv.ParseInt(s, 2, 0)
	if err != nil {
		panic(err)
	}

	fmt.Println("i is", reflect.TypeOf(i), i)
}

```

```go
s is string 1101
i is int64 13
```

---

```rust
fn main() {
    let s = "1101"; // binary digits
    
    let i = i32::from_str_radix(s, 2).expect("Not a binary number!");
    
    println!("{}", i);
}

```

`13`

<br>

### 252. <font color="0c0a3e">Conditional assignment</font>


> Assign to variable x the value "a" if calling the function condition returns true, or the value "b" otherwise.


*条件赋值*

```go
package main

import (
	"fmt"
)

func main() {
	var x string
	if condition() {
		x = "a"
	} else {
		x = "b"
	}

	fmt.Println(x)
}

func condition() bool {
	return "Scorates" == "dog"
}

```

`b`

---

```rust
x = if condition() { "a" } else { "b" };
```

<br>



### 258. <font color="0c0a3e">Convert list of strings to list of integers</font>


> Convert the string values from list a into a list of integers b.


*将字符串列表转换为整数列表*

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	a := []string{"11", "22", "33"}

	b := make([]int, len(a))
	var err error
	for i, s := range a {
		b[i], err = strconv.Atoi(s)
		if err != nil {
			panic(err)
		}
	}

	fmt.Println(b)
}

```

`[11 22 33]`

---

```rust
fn main() {
    let a: Vec<&str> = vec!["11", "-22", "33"];

    let b: Vec<i64> = a.iter().map(|x| x.parse::<i64>().unwrap()).collect();

    println!("{:?}", b);
}

```

`[11, -22, 33]`


<br>

### 259. <font color="0c0a3e">Split on several separators</font>

> Build list parts consisting of substrings of input string s, separated by any of the characters ',' (comma), '-' (dash), '_' (underscore).


*在几个分隔符上拆分*

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	s := "2021-03-11,linux_amd64"

	re := regexp.MustCompile("[,\\-_]")
	parts := re.Split(s, -1)
	
	fmt.Printf("%q", parts)
}
```

`["2021" "03" "11" "linux" "amd64"]`

---

```rust
fn main() {
    let s = "2021-03-11,linux_amd64";

    let parts: Vec<_> = s.split(&[',', '-', '_'][..]).collect();

    println!("{:?}", parts);
}

```

`["2021", "03", "11", "linux", "amd64"]`

<br>

### 266. <font color="0c0a3e">Repeating string</font>


> Assign to string s the value of string v, repeated n times and write it out.<br>E.g. v="abc", n=5 ⇒ s="abcabcabcabcabc"


*重复字符串*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	v := "abc"
	n := 5

	s := strings.Repeat(v, n)

	fmt.Println(s)
}
```

`abcabcabcabcabc`

---

```rust
fn main() {
    let v = "abc";
    let n = 5;

    let s = v.repeat(n);
    println!("{}", s);
}
```

`abcabcabcabcabc`

<br>

