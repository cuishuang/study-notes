---
title: 'Rust vs Go:常用语法对比(5)'
date: 2021-09-06 23:51:58
tags: Rust
---


<br>

### 81. <font color="fb6107">Round floating point number to integer</font>


> Declare integer y and initialize it with the rounded value of floating point number x .
Ties (when the fractional part of x is exactly .5) must be rounded up (to positive infinity).

*按规则取整*

```go
package main

import (
	"fmt"
	"math"
)

func round(x float64) int {
	y := int(math.Floor(x + 0.5))
	return y
}

func main() {
	for _, x := range []float64{-1.1, -0.9, -0.5, -0.1, 0., 0.1, 0.5, 0.9, 1.1} {
		fmt.Printf("%5.1f %5d\n", x, round(x))
	}
}
```

```go
 -1.1    -1
 -0.9    -1
 -0.5     0
 -0.1     0
  0.0     0
  0.1     0
  0.5     1
  0.9     1
  1.1     1
```

---


```rust
fn main() {
    let x : f64 = 2.71828;
    let y = x.round() as i64;
    
    println!("{} {}", x, y);
}
```

`2.71828 3`

<br>

### 82. <font color="f3de2c">Count substring occurrences</font>

*统计子字符串出现次数*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "Romaromamam"
	t := "mam"

	x := strings.Count(s, t)

	fmt.Println(x)
}

```

`1`

---



```rust
fn main() {
    let s = "lorem ipsum lorem ipsum lorem ipsum lorem ipsum";
    let t = "ipsum";

    let c = s.matches(t).count();

    println!("{} occurrences", c);
}

```

**Disjoint matches: overlapping occurrences are not counted.**


`4 occurrences`


<br>

### 83. <font color="7cb518">Regex with character repetition</font>

> Declare regular expression r matching strings "http", "htttp", "httttp", etc.

*正则表达式匹配*重复字符

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	r := regexp.MustCompile("htt+p")

	for _, s := range []string{
		"hp",
		"htp",
		"http",
		"htttp",
		"httttp",
		"htttttp",
		"htttttp",
		"word htttp in a sentence",
	} {
		fmt.Println(s, "=>", r.MatchString(s))
	}
}
```

```go
hp => false
htp => false
http => true
htttp => true
httttp => true
htttttp => true
htttttp => true
word htttp in a sentence => true
```

---

```rust
extern crate regex;
use regex::Regex;

fn main() {
    let r = Regex::new(r"htt+p").unwrap();
    
    assert!(r.is_match("http"));
    assert!(r.is_match("htttp"));
    assert!(r.is_match("httttp"));
}
```

<br>

### 84. <font color="5c8001">Count bits set in integer binary representation</font>


<font size=1 color="orange">
Count number c of 1s in the integer i in base 2.

E.g. i=6 → c=2

</font>

*计算十进制整型的二进制表示中 1的个数*

```go
package main

import "fmt"

func PopCountUInt64(i uint64) (c int) {
	// bit population count, see
	// http://graphics.stanford.edu/~seander/bithacks.html#CountBitsSetParallel
	i -= (i >> 1) & 0x5555555555555555
	i = (i>>2)&0x3333333333333333 + i&0x3333333333333333
	i += i >> 4
	i &= 0x0f0f0f0f0f0f0f0f
	i *= 0x0101010101010101
	return int(i >> 56)
}

func PopCountUInt32(i uint32) (n int) {
	// bit population count, see
	// http://graphics.stanford.edu/~seander/bithacks.html#CountBitsSetParallel
	i -= (i >> 1) & 0x55555555
	i = (i>>2)&0x33333333 + i&0x33333333
	i += i >> 4
	i &= 0x0f0f0f0f
	i *= 0x01010101
	return int(i >> 24)
}

func main() {
	for i := uint64(0); i < 16; i++ {
		c := PopCountUInt64(i)
		fmt.Printf("%4d %04[1]b %d\n", i, c)
	}

	for i := uint32(0); i < 16; i++ {
		c := PopCountUInt32(i)
		fmt.Printf("%4d %04[1]b %d\n", i, c)
	}
}
```


```go
   0 0000 0
   1 0001 1
   2 0010 1
   3 0011 2
   4 0100 1
   5 0101 2
   6 0110 2
   7 0111 3
   8 1000 1
   9 1001 2
  10 1010 2
  11 1011 3
  12 1100 2
  13 1101 3
  14 1110 3
  15 1111 4
   0 0000 0
   1 0001 1
   2 0010 1
   3 0011 2
   4 0100 1
   5 0101 2
   6 0110 2
   7 0111 3
   8 1000 1
   9 1001 2
  10 1010 2
  11 1011 3
  12 1100 2
  13 1101 3
  14 1110 3
  15 1111 4
```

*This was useful only before go 1.9.
See math/bits.OnesCount instead*

or

```go
package main

import (
	"fmt"
	"math/bits"
)

func main() {
	for i := uint(0); i < 16; i++ {
		c := bits.OnesCount(i)
		fmt.Printf("%4d %04[1]b %d\n", i, c)
	}
}

```


```go
   0 0000 0
   1 0001 1
   2 0010 1
   3 0011 2
   4 0100 1
   5 0101 2
   6 0110 2
   7 0111 3
   8 1000 1
   9 1001 2
  10 1010 2
  11 1011 3
  12 1100 2
  13 1101 3
  14 1110 3
  15 1111 4
  ```


---

```rust
fn main() {
    println!("{}", 6usize.count_ones())
}
```

`2`

<br>

### 85. <font color="fbb02d">Check if integer addition will overflow</font>

*检查两个整型相加是否溢出*

```go
package main

import (
	"fmt"
	"math"
)

func willAddOverflow(a, b int64) bool {
	return a > math.MaxInt64-b
}

func main() {

	fmt.Println(willAddOverflow(11111111111111111, 2))

}

```

`false`

---

```rust
fn adding_will_overflow(x: usize, y: usize) -> bool {
    x.checked_add(y).is_none()
}

fn main() {
    {
        let (x, y) = (2345678, 9012345);

        let overflow = adding_will_overflow(x, y);

        println!(
            "{} + {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
    {
        let (x, y) = (2345678901, 9012345678);

        let overflow = adding_will_overflow(x, y);

        println!(
            "{} + {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
    {
        let (x, y) = (2345678901234, 9012345678901);

        let overflow = adding_will_overflow(x, y);

        println!(
            "{} + {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
    {
        let (x, y) = (23456789012345678, 90123456789012345);

        let overflow = adding_will_overflow(x, y);

        println!(
            "{} + {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
    {
        let (x, y) = (12345678901234567890, 9012345678901234567);

        let overflow = adding_will_overflow(x, y);

        println!(
            "{} + {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
}

```


```rust
2345678 + 9012345 doesn't overflow
2345678901 + 9012345678 doesn't overflow
2345678901234 + 9012345678901 doesn't overflow
23456789012345678 + 90123456789012345 doesn't overflow
12345678901234567890 + 9012345678901234567 overflows
```

<br>

### 86. <font color="bfbdc1">Check if integer multiplication will overflow</font>

*检查整型相乘是否溢出*

```go
package main

import (
	"fmt"
)

func multiplyWillOverflow(x, y uint64) bool {
	if x <= 1 || y <= 1 {
		return false
	}
	d := x * y
	return d/y != x
}

func main() {
	{
		var x, y uint64 = 2345, 6789
		if multiplyWillOverflow(x, y) {
			fmt.Println(x, "*", y, "overflows")
		} else {
			fmt.Println(x, "*", y, "doesn't overflow")
		}
	}
	{
		var x, y uint64 = 2345678, 9012345
		if multiplyWillOverflow(x, y) {
			fmt.Println(x, "*", y, "overflows")
		} else {
			fmt.Println(x, "*", y, "doesn't overflow")
		}
	}
	{
		var x, y uint64 = 2345678901, 9012345678
		if multiplyWillOverflow(x, y) {
			fmt.Println(x, "*", y, "overflows")
		} else {
			fmt.Println(x, "*", y, "doesn't overflow")
		}
	}
}

```

```go
2345 * 6789 doesn't overflow
2345678 * 9012345 doesn't overflow
2345678901 * 9012345678 overflows
```

---


```rust
fn main() {
    {
        let (x, y) = (2345, 6789);

        let overflow = multiply_will_overflow(x, y);

        println!(
            "{} * {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
    {
        let (x, y) = (2345678, 9012345);

        let overflow = multiply_will_overflow(x, y);

        println!(
            "{} * {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
    {
        let (x, y) = (2345678901, 9012345678);

        let overflow = multiply_will_overflow(x, y);

        println!(
            "{} * {} {}",
            x,
            y,
            if overflow {
                "overflows"
            } else {
                "doesn't overflow"
            }
        );
    }
}

fn multiply_will_overflow(x: i64, y: i64) -> bool {
    x.checked_mul(y).is_none()
}
```

```rust
2345 * 6789 doesn't overflow
2345678 * 9012345 doesn't overflow
2345678901 * 9012345678 overflows
```

<br>

### 87. <font color="37323e">Stop program</font>

> Exit immediatly.<br> If some extra cleanup work is executed by the program runtime (not by the OS itself), describe it.


*停止程序,立即退出。*

```go
package main

import "os"

func main() {

	os.Exit(1)

	print(2222)
}

```

---



```rust
fn main() {
    std::process::exit(1);
    println!("42");
}
```

<br>

### 88. <font color="deb841">Allocate 1M bytes</font>

*分配1M内存*

```go
package main

import "fmt"

func main() {
	buf := make([]byte, 1000000)

	for i, b := range buf {
		if b != 0 {
			fmt.Println("Found unexpected value", b, "at position", i)
		}
	}
	fmt.Println("Buffer was correctly initialized with zero values.")
}
```

`Buffer was correctly initialized with zero values.`

---

```rust
fn main() {
    let buf: Vec<u8> = Vec::with_capacity(1024 * 1024);
    println!("{:?}", buf.capacity());
}
```

`1048576`

<br>

### 89. <font color="580aff">Handle invalid argument</font>

*处理无效参数*

```go
package main

import "fmt"

// NewSquareMatrix creates a N-by-N matrix
func NewSquareMatrix(N int) ([][]float64, error) {
	if N < 0 {
		return nil, fmt.Errorf("Invalid size %d: order cannot be negative", N)
	}
	matrix := make([][]float64, N)
	for i := range matrix {
		matrix[i] = make([]float64, N)
	}
	return matrix, nil
}

func main() {
	N1 := 3
	matrix1, err1 := NewSquareMatrix(N1)
	if err1 == nil {
		fmt.Println(matrix1)
	} else {
		fmt.Println(err1)
	}

	N2 := -2
	matrix2, err2 := NewSquareMatrix(N2)
	if err2 == nil {
		fmt.Println(matrix2)
	} else {
		fmt.Println(err2)
	}
}

```

```go
[[0 0 0] [0 0 0] [0 0 0]]
Invalid size -2: order cannot be negative
```

---

```rust
#[derive(Debug, PartialEq, Eq)]
enum CustomError { InvalidAnswer }

fn do_stuff(x: i32) -> Result<i32, CustomError> {
    if x != 42 {
%2
```


<br>

### 90. <font color="de9e36">Read-only outside</font>

*外部只读*

```go
type Foo struct {
	x int
}

func (f *Foo) X() int {
	return f.x
}
```

```go
x is private, because it is not capitalized.
(*Foo).X is a public getter (a read accessor).
```

---

```rust
struct Foo {
    x: usize
}

impl Foo {
    pub fn new(x: usize) -> Self {
        Foo { x }
    }

    pub fn x<'a>(&'a self) -> &'a usize {
        &self.x
    }
}
```


<br>

### 91. <font color="031d44">Load JSON file into struct</font>


*json转结构体*

```go
package main

import "fmt"
import "io/ioutil"
import "encoding/json"

func readJSONFile() error {
	var x Person

	buffer, err := ioutil.ReadFile(filename)
	if err != nil {
		return err
	}
	err = json.Unmarshal(buffer, &x)
	if err != nil {
		return err
	}

	fmt.Println(x)
	return nil
}

func main() {
	err := readJSONFile()
	if err != nil {
		panic(err)
	}
}

type Person struct {
	FirstName string
	Age       int
}

const filename = "/tmp/data.json"

func init() {
	err := ioutil.WriteFile(filename, []byte(`
		{
			"FirstName":"Napoléon",
			"Age": 51 
		}`), 0644)
	if err != nil {
		panic(err)
	}
}
```

`{Napoléon 51}`

or


```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"os"
)

func readJSONFile() error {
	var x Person

	r, err := os.Open(filename)
	if err != nil {
		return err
	}
	decoder := json.NewDecoder(r)
	err = decoder.Decode(&x)
	if err != nil {
		return err
	}

	fmt.Println(x)
	return nil
}

func main() {
	err := readJSONFile()
	if err != nil {
		panic(err)
	}
}

type Person struct {
	FirstName string
	Age       int
}

const filename = "/tmp/data.json"

func init() {
	err := ioutil.WriteFile(filename, []byte(`
		{
			"FirstName":"Napoléon",
			"Age": 51 
		}`), 0644)
	if err != nil {
		panic(err)
	}
}
```

`{Napoléon 51}`


---


```rust
#[macro_use] extern crate serde_derive;
extern crate serde_json;
use std::fs::File;
let x = ::serde_json::from_reader(File::open("data.json")?)?;
```


<br>

### 92. <font color="04395e">Save object into JSON file</font>

*将json对象写入文件*

```go
package main

import "fmt"
import "io/ioutil"
import "encoding/json"

func writeJSONFile() error {
	x := Person{
		FirstName: "Napoléon",
		Age:       51,
	}

	buffer, err := json.MarshalIndent(x, "", "  ")
	if err != nil {
		return err
	}
	return ioutil.WriteFile(filename, buffer, 0644)
}

func main() {
	err := writeJSONFile()
	if err != nil {
		panic(err)
	}
	fmt.Println("Done.")
}

type Person struct {
	FirstName string
	Age       int
}

const filename = "/tmp/data.json"

```

*json.MarshalIndent is more human-readable than json.Marshal.*

`Done.`

---



```rust
extern crate serde_json;
#[macro_use] extern crate serde_derive;

use std::fs::File;
::serde_json::to_writer(&File::create("data.json")?, &x)?
```


<br>

### 93. <font color="70a288">Pass a runnable procedure as parameter</font>

> Implement procedure control which receives one parameter f, and runs f.

*以函数作为参数*

```go
package main

import "fmt"

func main() {
	control(greet)
}

func control(f func()) {
	fmt.Println("Before f")
	f()
	fmt.Println("After f")
}

func greet() {
	fmt.Println("Hello, developers")
}

```


*Go supports first class functions, higher-order functions, user-defined function types, function literals, and closures.*

```go
Before f
Hello, developers
After f
```




---


```rust
fn control(f: impl Fn()) {
    f();
}

fn hello() {
    println!("Hello,");
}

fn main() {
    control(hello);
    control(|| { println!("Is there anybody in there?"); });
}

```


```rust
Hello,
Is there anybody in there?
```

<br>

### 94. <font color="dab785">Print type of variable</font>

*打印变量的类型*

```go
package main

import (
	"fmt"
	"os"
	"reflect"
)

func main() {
	var x interface{}

	x = "Hello"
	fmt.Println(reflect.TypeOf(x))

	x = 4
	fmt.Println(reflect.TypeOf(x))

	x = os.NewFile(0777, "foobar.txt")
	fmt.Println(reflect.TypeOf(x))
}

```

```go
string
int
*os.File
```

or

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	var x interface{}

	x = "Hello"
	fmt.Printf("%T", x)
	fmt.Println()

	x = 4
	fmt.Printf("%T", x)
	fmt.Println()

	x = os.NewFile(0777, "foobar.txt")
	fmt.Printf("%T", x)
	fmt.Println()
}
```

```go
string
int
*os.File
```



---

```rust
#![feature(core_intrinsics)]

fn type_of<T>(_: &T) -> String {
    format!("{}", std::intrinsics::type_name::<T>())
}

fn main() {
    let x: i32 = 1;
    println!("{}", type_of(&x));
}
```

`i32`

<br>

### 95. <font color="d5896f">Get file size</font>

*获取文件的大小*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func main() {
	err := printSize("file.txt")
	if err != nil {
		panic(err)
	}
}

func printSize(path string) error {
	info, err := os.Stat(path)
	if err != nil {
		return err
	}
	x := info.Size()

	fmt.Println(x)
	return nil
}

func init() {
	// The file will only contains the characters "Hello", no newlines.
	buffer := []byte("Hello")
	err := ioutil.WriteFile("file.txt", buffer, 0644)
	if err != nil {
		panic(err)
	}
}

```

`5`

---

```rust
use std::fs;

fn filesize(path: &str) -> Result<u64, std::io::Error> {
    let x = fs::metadata(path)?.len();
    Ok(x)
}

fn main() {
    let path = "/etc/hosts";
    let x = filesize(path);
    println!("{}: {:?} bytes", path, x.unwrap());
}
```

`/etc/hosts: 150 bytes`

or


```rust
use std::path::Path;

fn filesize(path: &std::path::Path) -> Result<u64, std::io::Error> {
    let x = path.metadata()?.len();
    Ok(x)
}

fn main() {
    let path = Path::new("/etc/hosts");
    let x = filesize(path);
    println!("{:?}: {:?} bytes", path, x.unwrap());
}

```

`"/etc/hosts": 150 bytes`

<br>

### 96. <font color="e8d6cb">Check string prefix</font>


> Set boolean b to true if string s starts with prefix prefix, false otherwise.


*检查两个字符串前缀是否一致*

```go
package main

import (
	"fmt"
	"strings"
)

func check(s, prefix string) {

	b := strings.HasPrefix(s, prefix)

	if b {
		fmt.Println(s, "starts with", prefix)
	} else {
		fmt.Println(s, "doesn't start with", prefix)
	}
}

func main() {
	check("bar", "foo")
	check("foobar", "foo")
}

```

```go
bar doesn't start with foo
foobar starts with foo
```

---



```rust
fn main() {
    let s = "bananas";
    let prefix = "bana";

    let b = s.starts_with(prefix);

    println!("{:?}", b);
}

```

`true`

<br>

### 97. <font color="d0ada7">Check string suffix</font>


> Set boolean b to true if string s ends with string suffix, false otherwise.

*检查字符串后缀*

```go
package main

import (
	"fmt"
	"strings"
)

func check(s, suffix string) {

	b := strings.HasSuffix(s, suffix)

	if b {
		fmt.Println(s, "ends with", suffix)
	} else {
		fmt.Println(s, "doesn't end with", suffix)
	}
}

func main() {
	check("foo", "bar")
	check("foobar", "bar")
}
```


```go
foo doesn't end with bar
foobar ends with bar
```

---


```rust
fn main() {
    let s = "bananas";
    let suffix = "nas";

    let b = s.ends_with(suffix);

    println!("{:?}", b);
}
```

`true`

<br>

### 98. <font color="ad6a6c">Epoch seconds to date object</font>

> Convert a timestamp ts (number of seconds in epoch-time) to a date with time d. E.g. 0 -> 1970-01-01 00:00:00

*时间戳转日期*

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ts := int64(1451606400)
	d := time.Unix(ts, 0)

	fmt.Println(d)
}
```


`2016-01-01 00:00:00 +0000 UTC`

---

```rust
extern crate chrono;
use chrono::prelude::*;

fn main() {
    let ts = 1451606400;
    let d = NaiveDateTime::from_timestamp(ts, 0);
    println!("{}", d);
}
```

`2016-01-01 00:00:00`

<br>

### 99. <font color="5d2e46">Format date YYYY-MM-DD</font>


> Assign to string x the value of fields (year, month, day) of date d, in format YYYY-MM-DD.


*时间格式转换*

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	d := time.Now()
	x := d.Format("2006-01-02")
	fmt.Println(x)

	// The output may be "2009-11-10" because the Playground's clock is fixed in the past.
}
```

`2009-11-10`


---

```rust
extern crate chrono;

use chrono::prelude::*;

fn main() {
    println!("{}", Utc::today().format("%Y-%m-%d"))
}

```

`2021-07-17`

<br>

### 100. <font color="b58db6">Sort by a comparator</font>

> Sort elements of array-like collection items, using a comparator c.



*根据某个字段排序*

```go
package main

import "fmt"
import "sort"

type Item struct {
	label string
	p     int
	lang  string
}

// c returns true if x is "inferior to" y (in a custom way)
func c(x, y Item) bool {
	return x.p < y.p
}

type ItemCSorter []Item

func (s ItemCSorter) Len() int           { return len(s) }
func (s ItemCSorter) Less(i, j int) bool { return c(s[i], s[j]) }
func (s ItemCSorter) Swap(i, j int)      { s[i], s[j] = s[j], s[i] }

func sortItems(items []Item) {
	sorter := ItemCSorter(items)
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
**c has type func(Item, Item) bool.**

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

type ItemsSorter struct {
	items []Item
	c     func(x, y Item) bool
}

func (s ItemsSorter) Len() int           { return len(s.items) }
func (s ItemsSorter) Less(i, j int) bool { return s.c(s.items[i], s.items[j]) }
func (s ItemsSorter) Swap(i, j int)      { s.items[i], s.items[j] = s.items[j], s.items[i] }

func sortItems(items []Item, c func(x, y Item) bool) {
	sorter := ItemsSorter{
		items,
		c,
	}
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

	c := func(x, y Item) bool {
		return x.p < y.p
	}
	sortItems(items, c)

	fmt.Println("Sorted: ", items)
}

```

**ItemsSorter contains c, which can be any comparator decided at runtime.**

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

// c returns true if x is "inferior to" y (in a custom way)
func c(x, y Item) bool {
	return x.p < y.p
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
	
	sort.Slice(items, func(i, j int) bool {
		return c(items[i], items[j])
	})
	
	fmt.Println("Sorted: ", items)
}
```

**Since Go 1.8, a single func parameter is sufficient to sort a slice.**

```go
Unsorted:  [{twelve 12 english} {six 6 english} {eleven 11 english} {zero 0 english} {two 2 english}]
Sorted:  [{zero 0 english} {two 2 english} {six 6 english} {eleven 11 english} {twelve 12 english}]
```



---

```rust
fn main() {
    let mut items = [1, 7, 5, 2, 3];
    items.sort_by(i32::cmp);
    println!("{:?}", items);
}
```

`[1, 2, 3, 5, 7]`

<br>


