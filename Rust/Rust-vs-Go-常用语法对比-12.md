---
title: 'Rust vs Go:常用语法对比(12)'
date: 2021-09-13 22:43:58
tags: Rust
---


<br>

### 221. <font color="0c0a3e">Remove all non-digits characters</font>

> Create string t from string s, keeping only digit characters 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.


*删除所有非数字字符*

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	s := `height="168px"`

	re := regexp.MustCompile("[^\\d]")
	t := re.ReplaceAllLiteralString(s, "")

	fmt.Println(t)
}

```

`168`

---

```rust
fn main() {
    let t: String = "Today is the 14th of July"
        .chars()
        .filter(|c| c.is_digit(10))
        .collect();

    dbg!(t);
}
```

`[src/main.rs:7] t = "14"`

<br>


### 222. <font color="0c0a3e">Find first index of an element in list</font>

> Set i to the first index in list items at which the element x can be found, or -1 if items does not contain x.



*在列表中查找元素的第一个索引*

```go
package main

import (
	"fmt"
)

func main() {
	items := []string{"huey", "dewey", "louie"}
	x := "dewey"

	i := -1
	for j, e := range items {
		if e == x {
			i = j
			break
		}
	}

	fmt.Printf("Found %q at position %d in %q", x, i, items)
}
```


`Found "dewey" at position 1 in ["huey" "dewey" "louie"]`


---

```rust
fn main() {
    let items = ['A', '🎂', '㍗'];
    let x = '💩';

    match items.iter().position(|y| *y == x) {
        Some(i) => println!("Found {} at position {}.", x, i),
        None => println!("There is no {} in the list.", x),
    }
}

```

`There is no 💩 in the list.`

or

```rust
fn main() {
    let items = [42, -3, 12];

    {
        let x = 12;

        let i = items.iter().position(|y| *y == x).map_or(-1, |n| n as i32);

        println!("{} => {}", x, i)
    }

    {
        let x = 13;

        let i = items.iter().position(|y| *y == x).map_or(-1, |n| n as i32);

        println!("{} => {}", x, i)
    }
}

```

```rust
12 => 2
13 => -1
```

<br>

### 223. <font color="0c0a3e">for else loop</font>

> Loop through list items checking a condition. Do something else if no matches are found.<br>A typical use case is looping through a series of containers looking for one that matches a condition. If found, an item is inserted; otherwise, a new container is created.<br>These are mostly used as an inner nested loop, and in a location where refactoring inner logic into a separate function reduces clarity.



*for else循环*

```go
package main

import (
	"fmt"
)

func main() {
	items := []string{"foo", "bar", "baz", "qux"}

	for _, item := range items {
		if item == "baz" {
			fmt.Println("found it")
			goto forelse
		}
	}
	{
		fmt.Println("never found it")
	}
        forelse:
}
```

`found it`

---

```rust
fn main() {
    let items: &[&str] = &["foo", "bar", "baz", "qux"];

    let mut found = false;
    for item in items {
        if item == &"baz" {
            println!("found it");
            found = true;
            break;
        }
    }
    if !found {
        println!("never found it");
    }
}

```

`found it`

or

```rust
fn main() {
     let items: &[&str] = &["foo", "bar", "baz", "qux"];

    if let None = items.iter().find(|&&item| item == "rockstar programmer") {
        println!("NotFound");
    };
}
```

`NotFound`

or

```rust
fn main() {
    let items: &[&str] = &["foo", "bar", "baz", "qux"];

    items
        .iter()
        .find(|&&item| item == "rockstar programmer")
        .or_else(|| {
            println!("NotFound");
            Some(&"rockstar programmer")
        });
}
```

`NotFound`


<br>

### 224. <font color="0c0a3e">Add element to the beginning of the list</font>


> Insert element x at the beginning of list items.


*将元素添加到列表的开头*

```go
package main

import (
	"fmt"
)

type T int

func main() {
	items := []T{42, 1337}
	var x T = 7
	
	items = append([]T{x}, items...)

	fmt.Println(items)
}

```

`[7 42 1337]`

or

```go
package main

import (
	"fmt"
)

type T int

func main() {
	items := []T{42, 1337}
	var x T = 7

	items = append(items, x)
	copy(items[1:], items)
	items[0] = x

	fmt.Println(items)
}
```

`[7 42 1337]`

---

```rust
use std::collections::VecDeque;

fn main() {
    let mut items = VecDeque::new();
    items.push_back(22);
    items.push_back(33);
    let x = 11;

    items.push_front(x);

    println!("{:?}", items);
}

```


`[11, 22, 33]`

<br>

### 225. <font color="0c0a3e">Declare and use an optional argument</font>

> Declare an optional integer argument x to procedure f, printing out "Present" and its value if it is present, "Not present" otherwise


*声明并使用可选参数*

```go
package main

func f(x ...int) {
	if len(x) > 0 {
		println("Present", x[0])
	} else {
		println("Not present")
	}
}

func main() {
	f()
	f(1)
}

```


*Go does not have optional arguments, but to some extend, they can be mimicked with a variadic parameter.
x is a variadic parameter, which must be the last parameter for the function f.
Strictly speaking, x is a list of integers, which might have more than one element. These additional elements are ignored.*



```go
Not present
Present 1
```

---

```rust
fn f(x: Option<()>) {
    match x {
        Some(x) => println!("Present {}", x),
        None => println!("Not present"),
    }
}
```

<br>

### 226. <font color="0c0a3e">Delete last element from list</font>

> Remove the last element from list items.


*从列表中删除最后一个元素*

```go
package main

import (
	"fmt"
)

func main() {
	items := []string{"banana", "apple", "kiwi"}
	fmt.Println(items)

	items = items[:len(items)-1]
	fmt.Println(items)
}
```

```go
[banana apple kiwi]
[banana apple]
```

---

```rust
fn main() {
    let mut items = vec![11, 22, 33];

    items.pop();

    println!("{:?}", items);
}

```

`[11, 22]`

<br>

### 227. <font color="0c0a3e">Copy list</font>

> Create new list y containing the same elements as list x.<br>Subsequent modifications of y must not affect x (except for the contents referenced by the elements themselves if they contain pointers).


*复制列表*

```go
package main

import (
	"fmt"
)

func main() {
	type T string
	x := []T{"Never", "gonna", "shower"}

	y := make([]T, len(x))
	copy(y, x)

	y[2] = "give"
	y = append(y, "you", "up")

	fmt.Println(x)
	fmt.Println(y)
}

```

```go
[Never gonna shower]
[Never gonna give you up]
```


---

```rust
fn main() {
    let mut x = vec![4, 3, 2];

    let y = x.clone();

    x[0] = 99;
    println!("x is {:?}", x);
    println!("y is {:?}", y);
}
```

```rust
x is [99, 3, 2]
y is [4, 3, 2]
```

<br>


### 228. <font color="0c0a3e">Copy a file</font>


> Copy the file at path src to dst.

*复制文件*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"os"
)

func main() {
	src, dst := "/tmp/file1", "/tmp/file2"

	err := copy(dst, src)
	if err != nil {
		log.Fatalln(err)
	}

	stat, err := os.Stat(dst)
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(dst, "exists, it has size", stat.Size(), "and mode", stat.Mode())
}

func copy(dst, src string) error {
	data, err := ioutil.ReadFile(src)
	if err != nil {
		return err
	}
	stat, err := os.Stat(src)
	if err != nil {
		return err
	}
	return ioutil.WriteFile(dst, data, stat.Mode())
}

func init() {
	data := []byte("Hello")
	err := ioutil.WriteFile("/tmp/file1", data, 0644)
	if err != nil {
		log.Fatalln(err)
	}
}

```

`/tmp/file2 exists, it has size 5 and mode -rw-r--r--`

or

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"os"
)

func main() {
	src, dst := "/tmp/file1", "/tmp/file2"

	err := copy(dst, src)
	if err != nil {
		log.Fatalln(err)
	}

	stat, err := os.Stat(dst)
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(dst, "exists, it has size", stat.Size(), "and mode", stat.Mode())
}

func copy(dst, src string) error {
	data, err := ioutil.ReadFile(src)
	if err != nil {
		return err
	}
	stat, err := os.Stat(src)
	if err != nil {
		return err
	}
	err = ioutil.WriteFile(dst, data, stat.Mode())
	if err != nil {
		return err
	}
	return os.Chmod(dst, stat.Mode())
}

func init() {
	data := []byte("Hello")
	err := ioutil.WriteFile("/tmp/file1", data, 0777)
	if err != nil {
		log.Fatalln(err)
	}
	err = os.Chmod("/tmp/file1", 0777)
	if err != nil {
		log.Fatalln(err)
	}
}

```

`/tmp/file2 exists, it has size 5 and mode -rwxrwxrwx`

or

```go
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"log"
	"os"
)

func main() {
	src, dst := "/tmp/file1", "/tmp/file2"

	err := copy(dst, src)
	if err != nil {
		log.Fatalln(err)
	}

	stat, err := os.Stat(dst)
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(dst, "exists, it has size", stat.Size(), "and mode", stat.Mode())
}

func copy(dst, src string) error {
	f, err := os.Open(src)
	if err != nil {
		return err
	}
	defer f.Close()
	stat, err := f.Stat()
	if err != nil {
		return err
	}
	g, err := os.OpenFile(dst, os.O_WRONLY|os.O_CREATE|os.O_TRUNC, stat.Mode())
	if err != nil {
		return err
	}
	defer g.Close()
	_, err = io.Copy(g, f)
	if err != nil {
		return err
	}
	return os.Chmod(dst, stat.Mode())
}

func init() {
	data := []byte("Hello")
	err := ioutil.WriteFile("/tmp/file1", data, 0777)
	if err != nil {
		log.Fatalln(err)
	}
	err = os.Chmod("/tmp/file1", 0777)
	if err != nil {
		log.Fatalln(err)
	}
}

```

`/tmp/file2 exists, it has size 5 and mode -rwxrwxrwx`


---

```rust
use std::fs;

fn main() {
    let src = "/etc/fstabZ";
    let dst = "fstab.bck";

    let r = fs::copy(src, dst);

    match r {
        Ok(v) => println!("Copied {:?} bytes", v),
        Err(e) => println!("error copying {:?} to {:?}: {:?}", src, dst, e),
    }
}

```


`error copying "/etc/fstabZ" to "fstab.bck": Os { code: 2, kind: NotFound, message: "No such file or directory" }`

<br>

### 231. <font color="0c0a3e">Test if bytes are a valid UTF-8 string</font>

> 	Set b to true if the byte sequence s consists entirely of valid UTF-8 character code points, false otherwise.


*测试字节是否是有效的UTF-8字符串*

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	{
		s := []byte("Hello, 世界")
		b := utf8.Valid(s)
		fmt.Println(b)
	}
	{
		s := []byte{0xff, 0xfe, 0xfd}
		b := utf8.Valid(s)
		fmt.Println(b)
	}
}

```


```go
true
false
```


---

```rust
fn main() {
    {
        let bytes = [0xc3, 0x81, 0x72, 0x76, 0xc3, 0xad, 0x7a];

        let b = std::str::from_utf8(&bytes).is_ok();
        println!("{}", b);
    }

    {
        let bytes = [0xc3, 0x81, 0x81, 0x76, 0xc3, 0xad, 0x7a];

        let b = std::str::from_utf8(&bytes).is_ok();
        println!("{}", b);
    }
}

```

```rust
true
false
```


<br>

### 234. <font color="0c0a3e">Encode bytes to base64</font>

> Assign to string s the standard base64 encoding of the byte array data, as specified by RFC 4648.

*将字节编码为base64*

```go
package main

import (
	"encoding/base64"
	"fmt"
)

func main() {
	data := []byte("Hello world")
	s := base64.StdEncoding.EncodeToString(data)
	fmt.Println(s)
}

```

`SGVsbG8gd29ybGQ=`

---

```rust
//use base64;

fn main() {
    let d = "Hello, World!";

    let b64txt = base64::encode(d);
    println!("{}", b64txt);
}
```

`SGVsbG8sIFdvcmxkIQ==`

<br>

### 235. <font color="0c0a3e">Decode base64</font>

> Assign to byte array data the bytes represented by the base64 string s, as specified by RFC 4648.

*解码base64*

```go
package main

import (
	"encoding/base64"
	"fmt"
)

func main() {
	str := "SGVsbG8gd29ybGQ="

	data, err := base64.StdEncoding.DecodeString(str)
	if err != nil {
		fmt.Println("error:", err)
		return
	}

	fmt.Printf("%q\n", data)
}
```

`"Hello world"`

---

```rust
//use base64;

fn main() {
    let d = "SGVsbG8sIFdvcmxkIQ==";

    let bytes = base64::decode(d).unwrap();
    println!("Hex: {:x?}", bytes);
    println!("Txt: {}", std::str::from_utf8(&bytes).unwrap());
}

```

```rust
Hex: [48, 65, 6c, 6c, 6f, 2c, 20, 57, 6f, 72, 6c, 64, 21]
Txt: Hello, World!
```

<br>

### 237. <font color="0c0a3e">Xor integers</font>

> Assign to c the result of (a xor b)


**异或运算**

*异或整数*

```go
package main

import (
	"fmt"
)

func main() {
	a, b := 230, 42
	c := a ^ b

	fmt.Printf("a is %12b\n", a)
	fmt.Printf("b is %12b\n", b)
	fmt.Printf("c is %12b\n", c)
	fmt.Println("c ==", c)
}

```

```go
a is     11100110
b is       101010
c is     11001100
c == 204
```


or


```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	a, b := big.NewInt(230), big.NewInt(42)
	c := new(big.Int)
	c.Xor(a, b)

	fmt.Printf("a is %12b\n", a)
	fmt.Printf("b is %12b\n", b)
	fmt.Printf("c is %12b\n", c)
	fmt.Println("c ==", c)
}
```

```go
a is     11100110
b is       101010
c is     11001100
c == 204
```




---

```rust
fn main() {
    let a = 230;
    let b = 42;
    let c = a ^ b;

    println!("{}", c);
}
```

`204`

<br>



### 238. <font color="0c0a3e">Xor byte arrays</font>


> Write in a new byte array c the xor result of byte arrays a and b.<br>a and b have the same size.


*异或字节数组*

```go
package main

import (
	"fmt"
)

func main() {
	a, b := []byte("Hello"), []byte("world")

	c := make([]byte, len(a))
	for i := range a {
		c[i] = a[i] ^ b[i]
	}

	fmt.Printf("a is %08b\n", a)
	fmt.Printf("b is %08b\n", b)
	fmt.Printf("c is %08b\n", c)
	fmt.Println("c ==", c)
	fmt.Printf("c as string would be %q\n", string(c))
}

```

```go
a is [01001000 01100101 01101100 01101100 01101111]
b is [01110111 01101111 01110010 01101100 01100100]
c is [00111111 00001010 00011110 00000000 00001011]
c == [63 10 30 0 11]
c as string would be "?\n\x1e\x00\v"
```

or

```go
package main

import (
	"fmt"
)

type T [5]byte

func main() {
	var a, b T
	copy(a[:], "Hello")
	copy(b[:], "world")

	var c T
	for i := range a {
		c[i] = a[i] ^ b[i]
	}

	fmt.Printf("a is %08b\n", a)
	fmt.Printf("b is %08b\n", b)
	fmt.Printf("c is %08b\n", c)
	fmt.Println("c ==", c)
	fmt.Printf("c as string would be %q\n", string(c[:]))
}
```

```go
a is [01001000 01100101 01101100 01101100 01101111]
b is [01110111 01101111 01110010 01101100 01100100]
c is [00111111 00001010 00011110 00000000 00001011]
c == [63 10 30 0 11]
c as string would be "?\n\x1e\x00\v"
```


---

```rust
fn main() {
    let a: &[u8] = "Hello".as_bytes();
    let b: &[u8] = "world".as_bytes();

    let c: Vec<_> = a.iter().zip(b).map(|(x, y)| x ^ y).collect();

    println!("{:?}", c);
}
```


`[63, 10, 30, 0, 11]`

<br>

### 239. <font color="0c0a3e">Find first regular expression match</font>


> Assign to string x the first word of string s consisting of exactly 3 digits, or the empty string if no such match exists.<br>A word containing more digits, or 3 digits as a substring fragment, must not match.


*查找第一个正则表达式匹配项*

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	re := regexp.MustCompile(`\b\d\d\d\b`)
	for _, s := range []string{
		"",
		"12",
		"123",
		"1234",
		"I have 12 goats, 3988 otters, 224 shrimps and 456 giraffes",
		"See p.456, for word boundaries",
	} {
		x := re.FindString(s)
		fmt.Printf("%q -> %q\n", s, x)
	}
}
```

```go
"" -> ""
"12" -> ""
"123" -> "123"
"1234" -> ""
"I have 12 goats, 3988 otters, 224 shrimps and 456 giraffes" -> "224"
"See p.456, for word boundaries" -> "456"
```

---

```rust
use regex::Regex;

fn main() {
    let sentences = vec![
        "",
        "12",
        "123",
        "1234",
        "I have 12 goats, 3988 otters, 224 shrimps and 456 giraffes",
        "See p.456, for word boundaries",
    ];
    for s in sentences {
        let re = Regex::new(r"\b\d\d\d\b").expect("failed to compile regex");
        let x = re.find(s).map(|x| x.as_str()).unwrap_or("");
        println!("[{}] -> [{}]", &s, &x);
    }
}

```

```rust
[] -> []
[12] -> []
[123] -> [123]
[1234] -> []
[I have 12 goats, 3988 otters, 224 shrimps and 456 giraffes] -> [224]
[See p.456, for word boundaries] -> [456]
```



<br>

### 240. <font color="0c0a3e">Sort 2 lists together</font>

> Lists a and b have the same length. Apply the same permutation to a and b to have them sorted based on the values of a.


*将两个列表排序在一起.列表a和b的长度相同。对a和b应用相同的排列，根据a的值对它们进行排序。*

```go
package main

import (
	"fmt"
	"sort"
)

type K int
type T string

type sorter struct {
	k []K
	t []T
}

func (s *sorter) Len() int {
	return len(s.k)
}

func (s *sorter) Swap(i, j int) {
	// Swap affects 2 slices at once.
	s.k[i], s.k[j] = s.k[j], s.k[i]
	s.t[i], s.t[j] = s.t[j], s.t[i]
}

func (s *sorter) Less(i, j int) bool {
	return s.k[i] < s.k[j]
}

func main() {
	a := []K{9, 3, 4, 8}
	b := []T{"nine", "three", "four", "eight"}

	sort.Sort(&sorter{
		k: a,
		t: b,
	})

	fmt.Println(a)
	fmt.Println(b)
}

```

```go
[3 4 8 9]
[three four eight nine]
```

---

```rust
fn main() {
    let a = vec![30, 20, 40, 10];
    let b = vec![101, 102, 103, 104];

    let mut tmp: Vec<_> = a.iter().zip(b).collect();
    tmp.as_mut_slice().sort_by_key(|(&x, _y)| x);
    let (aa, bb): (Vec<i32>, Vec<i32>) = tmp.into_iter().unzip();

    println!("{:?}, {:?}", aa, bb);
}
```

`[10, 20, 30, 40], [104, 102, 101, 103]`

<br>

