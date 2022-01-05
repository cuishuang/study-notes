---
title: 'Rust vs Go:常用语法对比(8)'
date: 2021-09-09 19:43:58
tags: Rust
---


<br>

### 141. <font color="0c0a3e">Iterate in sequence over two lists</font>

> Iterate in sequence over the elements of the list items1 then items2. For each iteration print the element.


*依次迭代两个列表
依次迭代列表项1和项2的元素。每次迭代打印元素。*

```go
package main

import (
	"fmt"
)

func main() {
	items1 := []string{"a", "b", "c"}
	items2 := []string{"A", "B", "C"}

	for _, v := range items1 {
		fmt.Println(v)
	}
	for _, v := range items2 {
		fmt.Println(v)
	}
}

```


```go
a
b
c
A
B
C
```

---

```rust
fn main() {
    let item1 = vec!["1", "2", "3"];
    let item2 = vec!["a", "b", "c"];
    for i in item1.iter().chain(item2.iter()) {
        print!("{} ", i);
    }
}
```

`1 2 3 a b c `

<br>


### 142. <font color="0c0a3e">Hexadecimal digits of an integer</font>

> Assign to string s the hexadecimal representation (base 16) of integer x.<br>E.g. 999 -> "3e7"


*将整数x的十六进制表示(16进制)赋给字符串s。*

```go
package main

import "fmt"
import "strconv"

func main() {
	x := int64(999)
	s := strconv.FormatInt(x, 16)

	fmt.Println(s)
}

```

`3e7`

or

```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	x := big.NewInt(999)
	s := fmt.Sprintf("%x", x)

	fmt.Println(s)
}

```

`3e7`

---

```rust
fn main() {
    let x = 999;

    let s = format!("{:X}", x);
    println!("{}", s);

    let s = format!("{:x}", x);
    println!("{}", s);
}

```

**{:X} produces uppercase hex.
{:x} produces lowercase hex.**


```rust
3E7
3e7
```

<br>

### 143. <font color="0c0a3e">Iterate alternatively over two lists</font>

> Iterate alternatively over the elements of the list items1 and items2. For each iteration, print the element.<br>Explain what happens if items1 and items2 have different size.

*交替迭代两个列表*

```go
package main

import (
	"fmt"
)

func main() {
	items1 := []string{"a", "b"}
	items2 := []string{"A", "B", "C"}

	for i := 0; i < len(items1) || i < len(items2); i++ {
		if i < len(items1) {
			fmt.Println(items1[i])
		}
		if i < len(items2) {
			fmt.Println(items2[i])
		}
	}
}

```

```go
a
A
b
B
C
```

---

```rust
extern crate itertools;
use itertools::izip;

fn main() {
    let items1 = [5, 15, 25];
    let items2 = [10, 20, 30];

    for pair in izip!(&items1, &items2) {
        println!("{}", pair.0);
        println!("{}", pair.1);
    }
}

```

```rust
5
10
15
20
25
30
```

<br>

### 144. <font color="0c0a3e">Check if file exists</font>

> 
Set boolean b to true if file at path fp exists on filesystem; false otherwise.<br>Beware that you should never do this and then in the next instruction assume the result is still valid, this is a race condition on any multitasking OS.


*检查文件是否存在*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func main() {
	fp := "foo.txt"
	_, err := os.Stat(fp)
	b := !os.IsNotExist(err)
	fmt.Println(fp, "exists:", b)

	fp = "bar.txt"
	_, err = os.Stat(fp)
	b = !os.IsNotExist(err)
	fmt.Println(fp, "exists:", b)
}

func init() {
	ioutil.WriteFile("foo.txt", []byte(`abc`), 0644)
}

```

**There's no specific existence check func in standard library, so we have to inspect an error return value.**


```go
foo.txt exists: true
bar.txt exists: false
```


---


```rust
fn main() {
    let fp = "/etc/hosts";
    let b = std::path::Path::new(fp).exists();
    println!("{}: {}", fp, b);

    let fp = "/etc/kittens";
    let b = std::path::Path::new(fp).exists();
    println!("{}: {}", fp, b);
}

```

```rust
/etc/hosts: true
/etc/kittens: false
```

<br>

### 145. <font color="0c0a3e">Print log line with datetime</font>

> Print message msg, prepended by current date and time.<br>Explain what behavior is idiomatic: to stdout or stderr, and what the date format is.


*打印带时间的日志*

```go
package main

import "log"

func main() {
	msg := "Hello, playground"
	log.Println(msg)

	// The date is fixed in the past in the Playground, never mind.
}

// See http://www.programming-idioms.org/idiom/145/print-log-line-with-date/1815/go

```

`2009/11/10 23:00:00 Hello, playground`

---

```rust
fn main() {
    let msg = "Hello";
    eprintln!("[{}] {}", humantime::format_rfc3339_seconds(std::time::SystemTime::now()), msg);
}
```

`[2021-07-17T07:14:03Z] Hello`

<br>

### 146. <font color="0c0a3e">Convert string to floating point number</font>

> Extract floating point value f from its string representation s

*字符串转换为浮点型*

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s := "3.1415926535"

	f, err := strconv.ParseFloat(s, 64)
	fmt.Printf("%T, %v, err=%v\n", f, f, err)
}

//
// http://www.programming-idioms.org/idiom/146/convert-string-to-floating-point-number/1819/go
//

```

`float64, 3.1415926535, err=<nil>`

---

```rust
fn main() {
    let s = "3.14159265359";
    let f = s.parse::<f32>().unwrap();
    
    println!("{}² = {}" , f, f * f);
}
```

`3.1415927² = 9.869605`


or

```rust
fn main() {
    let s = "3.14159265359";
    let f: f32 = s.parse().unwrap();

    println!("{}² = {}", f, f * f);
}
```

`3.1415927² = 9.869605`


<br>

### 147. <font color="0c0a3e">Remove all non-ASCII characters</font>


> 	Create string t from string s, keeping only ASCII characters


*移除所有的非ASCII字符*

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	s := "dæmi : пример : příklad : thí dụ"

	re := regexp.MustCompile("[[:^ascii:]]")
	t := re.ReplaceAllLiteralString(s, "")

	fmt.Println(t)
}

```

`dmi :  : pklad : th d`

or

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	s := "5#∑∂ƒ∞645eyfu"
	t := strings.Map(func(r rune) rune {
		if r > unicode.MaxASCII {
			return -1
		}
		return r
	}, s)
	fmt.Println(t)
}

```

`5#645eyfu`


---

```rust
fn main() {
    println!("{}", "do👍ot".replace(|c: char| !c.is_ascii(), ""))
}
```

`doot`

or

```rust
fn main() {
    println!("{}", "do👍ot".replace(|c: char| !c.is_ascii(), ""))
}
```

`doot`


<br>


### 148. <font color="0c0a3e">Read list of integers from stdin</font>


> Read a list of integer numbers from the standard input, until EOF.


*从stdin(标准输入)中读取整数列表*

```go
package main

import (
	"bufio"
	"fmt"
	"log"
	"strconv"
	"strings"
)

func main() {
	var ints []int
	s := bufio.NewScanner(osStdin)
	s.Split(bufio.ScanWords)
	for s.Scan() {
		i, err := strconv.Atoi(s.Text())
		if err == nil {
			ints = append(ints, i)
		}
	}
	if err := s.Err(); err != nil {
		log.Fatal(err)
	}
	fmt.Println(ints)
}

// osStdin simulates os.Stdin
var osStdin = strings.NewReader(`
11
22
33  `)

```

`[11 22 33]`

---

```rust
use std::{
    io::{self, Read},
    str::FromStr,
};

// dummy io::stdin
fn io_stdin() -> impl Read {
    "123
456
789"
    .as_bytes()
}

fn main() -> io::Result<()> {
    let mut string = String::new();
    io_stdin().read_to_string(&mut string)?;
    let result = string
        .lines()
        .map(i32::from_str)
        .collect::<Result<Vec<_>, _>>();
    println!("{:#?}", result);
    Ok(())
}
```

```rust
Ok(
    [
        123,
        456,
        789,
    ],
)
```


<br>

### 150. <font color="0c0a3e">Remove trailing slash</font>


> Remove last character from string p, if this character is a slash /.

*去除末尾的 /*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	p := "/usr/bin/"

	p = strings.TrimSuffix(p, "/")

	fmt.Println(p)
}
```

`/usr/bin`

---

```rust
fn main() {
    let mut p = String::from("Dddd/");
    if p.ends_with('/') {
        p.pop();
    }
    println!("{}", p);
}
```

`Dddd`

<br>

### 151. <font color="0c0a3e">Remove string trailing path separator</font>


> Remove last character from string p, if this character is the file path separator of current platform.<br>Note that this also transforms unix root path "/" into the empty string!


*删除字符串尾部路径分隔符*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
	"strings"
)

func main() {
	p := somePath()
	fmt.Println(p)

	sep := fmt.Sprintf("%c", os.PathSeparator)
	p = strings.TrimSuffix(p, sep)

	fmt.Println(p)
}

func somePath() string {
	dir, err := ioutil.TempDir("", "")
	if err != nil {
		panic(err)
	}
	p := fmt.Sprintf("%s%c%s%c", dir, os.PathSeparator, "foobar", os.PathSeparator)
	return p
}

```

```go
/tmp/067319278/foobar/
/tmp/067319278/foobar
```

or

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
	"path/filepath"
	"strings"
)

func main() {
	p := somePath()
	fmt.Println(p)

	sep := fmt.Sprintf("%c", filepath.Separator)
	p = strings.TrimSuffix(p, sep)

	fmt.Println(p)
}

func somePath() string {
	dir, err := ioutil.TempDir("", "")
	if err != nil {
		panic(err)
	}
	p := fmt.Sprintf("%s%c%s%c", dir, os.PathSeparator, "foobar", os.PathSeparator)
	return p
}
```


```go
/tmp/065654753/foobar/
/tmp/065654753/foobar
```

---

```rust
fn main() {
    {
        let p = "/tmp/";

        let p = if ::std::path::is_separator(p.chars().last().unwrap()) {
            &p[0..p.len() - 1]
        } else {
            p
        };

        println!("{}", p);
    }
    
    {
        let p = "/tmp";

        let p = if ::std::path::is_separator(p.chars().last().unwrap()) {
            &p[0..p.len() - 1]
        } else {
            p
        };

        println!("{}", p);
    }
}
```

```rust
/tmp
/tmp
```

or

```rust
fn main() {
    {
        let mut p = "/tmp/";

        p = p.strip_suffix(std::path::is_separator).unwrap_or(p);

        println!("{}", p);
    }
    
    {
        let mut p = "/tmp";

        p = p.strip_suffix(std::path::is_separator).unwrap_or(p);

        println!("{}", p);
    }
}
```

```rust
/tmp
/tmp
```

<br>

### 152. <font color="0c0a3e">Turn a character into a string</font>


> Create string s containing only the character c.

*将字符转换成字符串*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	var c rune = os.PathSeparator
	fmt.Printf("%c \n", c)

	s := fmt.Sprintf("%c", c)
	fmt.Printf("%#v \n", s)
}

```

```go
/ 
"/" 
```

---

```rust
fn main() {
    let c = 'a';
    
    let s = c.to_string();
    
    println!("{}", s);
}
```

`a`

<br>


### 153. <font color="0c0a3e">Concatenate string with integer</font>


> Create string t as the concatenation of string s and integer i.


*连接字符串和整数*

```go
package main

import (
	"fmt"
)

func main() {
	s := "Hello"
	i := 123

	t := fmt.Sprintf("%s%d", s, i)

	fmt.Println(t)
}

```

`Hello123`

---

```rust
fn main() {
    let s = "Foo";
    let i = 1;
    let t = format!("{}{}", s, i);
    
    println!("{}" , t);
}
```

`Foo1`


<br>

### 154. <font color="0c0a3e">Halfway between two hex color codes</font>


> Find color c, the average between colors c1, c2.<br>c, c1, c2 are strings of hex color codes: 7 chars, beginning with a number sign # .
Assume linear computations, ignore gamma corrections.

*求两个十六进制颜色代码的中间值*

```go
package main

import (
	"fmt"
	"strconv"
	"strings"
)

// For concision, halfway assume valid inputs.
// Caller must have explicitly checked that c1, c2 are well-formed color codes.
func halfway(c1, c2 string) string {
	r1, _ := strconv.ParseInt(c1[1:3], 16, 0)
	r2, _ := strconv.ParseInt(c2[1:3], 16, 0)
	r := (r1 + r2) / 2

	g1, _ := strconv.ParseInt(c1[3:5], 16, 0)
	g2, _ := strconv.ParseInt(c2[3:5], 16, 0)
	g := (g1 + g2) / 2

	b1, _ := strconv.ParseInt(c1[5:7], 16, 0)
	b2, _ := strconv.ParseInt(c2[5:7], 16, 0)
	b := (b1 + b2) / 2

	c := fmt.Sprintf("#%02X%02X%02X", r, g, b)
	return c
}

func main() {
	c1 := "#15293E"
	c2 := "#012549"

	if err := checkFormat(c1); err != nil {
		panic(fmt.Errorf("Wrong input %q: %v", c1, err))
	}
	if err := checkFormat(c2); err != nil {
		panic(fmt.Errorf("Wrong input %q: %v", c2, err))
	}

	c := halfway(c1, c2)
	fmt.Println("The average of", c1, "and", c2, "is", c)
}

func checkFormat(color string) error {
	if len(color) != 7 {
		return fmt.Errorf("Hex colors have exactly 7 chars")
	}
	if color[0] != '#' {
		return fmt.Errorf("Hex colors start with #")
	}
	isNotDigit := func(c rune) bool { return (c < '0' || c > '9') && (c < 'a' || c > 'f') }
	if strings.IndexFunc(strings.ToLower(color[1:]), isNotDigit) != -1 {
		return fmt.Errorf("Forbidden char")
	}
	return nil
}

```

`The average of #15293E and #012549 is #0B2743`



```go
package main

import (
	"fmt"
	"strconv"
	"strings"
)

// For concision, halfway assume valid inputs.
// Caller must have explicitly checked that c1, c2 are well-formed color codes.
func halfway(c1, c2 string) string {
	var buf [7]byte
	buf[0] = '#'
	for i := 0; i < 3; i++ {
		sub1 := c1[1+2*i : 3+2*i]
		sub2 := c2[1+2*i : 3+2*i]
		v1, _ := strconv.ParseInt(sub1, 16, 0)
		v2, _ := strconv.ParseInt(sub2, 16, 0)
		v := (v1 + v2) / 2
		sub := fmt.Sprintf("%02X", v)
		copy(buf[1+2*i:3+2*i], sub)
	}
	c := string(buf[:])

	return c
}

func main() {
	c1 := "#15293E"
	c2 := "#012549"

	if err := checkFormat(c1); err != nil {
		panic(fmt.Errorf("Wrong input %q: %v", c1, err))
	}
	if err := checkFormat(c2); err != nil {
		panic(fmt.Errorf("Wrong input %q: %v", c2, err))
	}

	c := halfway(c1, c2)
	fmt.Println("The average of", c1, "and", c2, "is", c)
}

func checkFormat(color string) error {
	if len(color) != 7 {
		return fmt.Errorf("Hex colors have exactly 7 chars")
	}
	if color[0] != '#' {
		return fmt.Errorf("Hex colors start with #")
	}
	isNotDigit := func(c rune) bool { return (c < '0' || c > '9') && (c < 'a' || c > 'f') }
	if strings.IndexFunc(strings.ToLower(color[1:]), isNotDigit) != -1 {
		return fmt.Errorf("Forbidden char")
	}
	return nil
}

```

`The average of #15293E and #012549 is #0B2743`



---

```rust
use std::str::FromStr;
use std::fmt;

#[derive(Debug)]
struct Colour {
    r: u8,
    g: u8,
    b: u8
}

#[derive(Debug)]
enum ColourError {
    MissingHash,
    InvalidRed,
    InvalidGreen,
    InvalidBlue
}

impl fmt::Display for Colour {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "#{:02x}{:02x}{:02x}", self.r, self.g, self.b)
    }
}

impl FromStr for Colour {
    type Err = ColourError;
    
    fn from_str(s: &str) -> Result<Self, Self::Err> {
        if !s.starts_with('#') {
            Err(ColourError::MissingHash)
        } else {
            Ok(Colour {
                r: u8::from_str_radix(&s[1..3], 16).map_err(|_| ColourError::InvalidRed)?,
                g: u8::from_str_radix(&s[3..5], 16).map_err(|_| ColourError::InvalidGreen)?,
                b: u8::from_str_radix(&s[5..7], 16).map_err(|_| ColourError::InvalidBlue)?
            })
        }
    }
}

fn mid_colour(c1: &str, c2: &str) -> Result<String, ColourError> {
    let c1 = c1.parse::<Colour>()?;
    let c2 = c2.parse::<Colour>()?;
    let c = Colour {
        r: (((c1.r as u16) + (c2.r as u16))/2) as u8,
        g: (((c1.g as u16) + (c2.g as u16))/2) as u8,
        b: (((c1.b as u16) + (c2.b as u16))/2) as u8
    };
    Ok(format!("{}", c))
}

fn main() {
    println!("{}", mid_colour("#15293E", "#012549").unwrap())
}
```

`#0b2743`

<br>

### 155. <font color="0c0a3e">Delete file</font>

> Delete from filesystem the file having path filepath.

*删除文件*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func main() {
	for _, filepath := range []string{
		"/tmp/foo.txt",
		"/tmp/bar.txt",
		"/tmp/foo.txt",
	} {
		err := os.Remove(filepath)
		if err == nil {
			fmt.Println("Removed", filepath)
		} else {
			fmt.Fprintln(os.Stderr, err)
		}
	}
}

func init() {
	err := ioutil.WriteFile("/tmp/foo.txt", []byte(`abc`), 0644)
	if err != nil {
		panic(err)
	}
}
```

```go
Removed /tmp/foo.txt
remove /tmp/bar.txt: no such file or directory
remove /tmp/foo.txt: no such file or directory
```

---

```rust
use std::fs;

fn main() {
    let filepath = "/tmp/abc";

    println!("Creating {}", filepath);
    let _file = fs::File::create(filepath);

    let b = std::path::Path::new(filepath).exists();
    println!("{} exists: {}", filepath, b);

    println!("Deleting {}", filepath);
    let r = fs::remove_file(filepath);
    println!("{:?}", r);

    let b = std::path::Path::new(filepath).exists();
    println!("{} exists: {}", filepath, b);
}
```

```rust
Creating /tmp/abc
/tmp/abc exists: true
Deleting /tmp/abc
Ok(())
/tmp/abc exists: false
```

<br>

### 156. <font color="0c0a3e">Format integer with zero-padding</font>

> Assign to string s the value of integer i in 3 decimal digits. Pad with zeros if i < 100. Keep all digits if i ≥ 1000.


*用零填充格式化整数*

```go
package main

import (
	"fmt"
)

func main() {
	for _, i := range []int{
		0,
		8,
		64,
		256,
		2048,
	} {
		s := fmt.Sprintf("%03d", i)
		fmt.Println(s)
	}
}

```

```go
000
008
064
256
2048
```


---

```rust
fn main() {
    let i = 1;
    let s = format!("{:03}", i);
    
    println!("{}", s);
    
    
    let i = 1000;
    let s = format!("{:03}", i);
    
    println!("{}", s);
}
```

```rust
001
1000
```

<br>


### 157. <font color="0c0a3e">Declare constant string</font>

> Initialize a constant planet with string value "Earth".


*声明常量字符串*

```go
package main

import (
	"fmt"
)

const planet = "Earth"

func main() {
	fmt.Println("We live on planet", planet)
}

```

`We live on planet Earth`

---

```rust
fn main() {
    const PLANET: &str = "Earth";
    
    println!("{}", PLANET);
}

```

`Earth`

<br>

### 158. <font color="0c0a3e">Random sublist</font>


> Create a new list y from randomly picking exactly k elements from list x.<br>


**It is assumed that x has at least k elements.
Each element must have same probability to be picked.
Each element from x must be picked at most once.
Explain if the original ordering is preserved or not.**


*随机子列表*

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	type T string

	x := []T{"Alice", "Bob", "Carol", "Dan", "Eve", "Frank", "Grace", "Heidi"}
	k := 4

	y := make([]T, k)
	perm := rand.Perm(len(x))
	for i, v := range perm[:k] {
		y[i] = x[v]
	}

	fmt.Printf("%q", y)
}

```

`["Frank" "Eve" "Carol" "Grace"]`

---

```rust
use rand::prelude::*;
let mut rng = &mut rand::thread_rng();
let y = x.choose_multiple(&mut rng, k).cloned().collect::<Vec<_>>();
```

<br>

### 159. <font color="0c0a3e">Trie</font>


> Define a Trie data structure, where entries have an associated value.
(Not all nodes are entries)

*前缀树/字典树*

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

type Trie struct {
	c        rune
	children map[rune]*Trie
	isLeaf   bool
	value    V
}

type V int

func main() {
	t := NewTrie(0)
	for s, v := range map[string]V{
		"to":  7,
		"tea": 3,
		"ted": 4,
		"ten": 12,
		"A":   15,
		"i":   11,
		"in":  5,
		"inn": 9,
	} {
		t.insert(s, v)
	}
	fmt.Println(t.startsWith("te", ""))
}

func NewTrie(c rune) *Trie {
	t := new(Trie)
	t.c = c
	t.children = map[rune]*Trie{}
	return t
}

func (t *Trie) insert(s string, value V) {
	if s == "" {
		t.isLeaf = true
		t.value = value
		return
	}
	c, tail := cut(s)
	child, exists := t.children[c]
	if !exists {
		child = NewTrie(c)
		t.children[c] = child
	}
	child.insert(tail, value)
}

func (t *Trie) startsWith(p string, accu string) []string {
	if t == nil {
		return nil
	}
	if p == "" {
		var result []string
		if t.isLeaf {
			result = append(result, accu)
		}
		for c, child := range t.children {
			rec := child.startsWith("", accu+string(c))
			result = append(result, rec...)
		}
		return result
	}
	c, tail := cut(p)
	return t.children[c].startsWith(tail, accu+string(c))
}

func cut(s string) (head rune, tail string) {
	r, size := utf8.DecodeRuneInString(s)
	return r, s[size:]
}
```

`[ten tea ted]`

---

```rust
struct Trie {
    val: String,
    nodes: Vec<Trie>
}
```

<br>

### 160. <font color="0c0a3e">Detect if 32-bit or 64-bit architecture</font>


> Execute f32() if platform is 32-bit, or f64() if platform is 64-bit.<br>This can be either a compile-time condition (depending on target) or a runtime detection.


*检测是32位还是64位架构*

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	if strconv.IntSize == 32 {
		f32()
	}
	if strconv.IntSize == 64 {
		f64()
	}
}

func f32() {
	fmt.Println("I am 32-bit")
}

func f64() {
	fmt.Println("I am 64-bit")
}
```

`I am 64-bit`

---

```rust
fn main() {
    match std::mem::size_of::<&char>() {
        4 => f32(),
        8 => f64(),
        _ => {}
    }
}

fn f32() {
    println!("I am 32-bit");
}

fn f64() {
    println!("I am 64-bit");
}
```

`I am 64-bit`

<br>

