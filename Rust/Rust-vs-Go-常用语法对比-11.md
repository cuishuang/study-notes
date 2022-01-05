---
title: 'Rust vs Go:常用语法对比(11)'
date: 2021-09-12 22:43:58
tags: Rust
---


<br>



### 202. <font color="0c0a3e">Sum of squares</font>


> Calculate the sum of squares s of data, an array of floating point values.


*计算平方和*

```go
package main

import (
	"math"
)

func main() {
	data := []float64{0.06, 0.82, -0.01, -0.34, -0.55}
	var s float64
	for _, d := range data {
		s += math.Pow(d, 2)
	}
	println(s)
}

```

`+1.094200e+000`

---

```rust
fn main() {
    let data: Vec<f32> = vec![2.0, 3.5, 4.0];

    let s = data.iter().map(|x| x.powi(2)).sum::<f32>();

    println!("{}", s);
}

```

`32.25`


<br>

### 205. <font color="0c0a3e">Get an environment variable</font>

> Read an environment variable with the name "FOO" and assign it to the string variable foo. If it does not exist or if the system does not support environment variables, assign a value of "none".


*获取环境变量*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	foo, ok := os.LookupEnv("FOO")
	if !ok {
		foo = "none"
	}

	fmt.Println(foo)
}

```

`none`

or

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	foo := os.Getenv("FOO")
	if foo == "" {
		foo = "none"
	}

	fmt.Println(foo)
}
```

`none`



---

```rust
use std::env;

fn main() {
    let foo;
    match env::var("FOO") {
        Ok(val) => foo = val,
        Err(_e) => foo = "none".to_string(),
    }
    println!("{}", foo);
    
    let user;
    match env::var("USER") {
        Ok(val) => user = val,
        Err(_e) => user = "none".to_string(),
    }
    println!("{}", user);
}

```

```rust
none
playground
```

or


```rust
use std::env;

fn main() {
    let foo = env::var("FOO").unwrap_or("none".to_string());
    println!("{}", foo);

    let user = env::var("USER").unwrap_or("none".to_string());
    println!("{}", user);
}

```

```rust
none
playground
```

or

```rust
use std::env;

fn main() {
    let foo = match env::var("FOO") {
        Ok(val) => val,
        Err(_e) => "none".to_string(),
    };
    println!("{}", foo);

    let user = match env::var("USER") {
        Ok(val) => val,
        Err(_e) => "none".to_string(),
    };
    println!("{}", user);
}

```

```rust
none
playground
```


or

```rust
use std::env;
if let Ok(tnt_root) = env::var("TNT_ROOT") {
     //
}
```


<br>

### 206. <font color="0c0a3e">Switch statement with strings</font>


> Execute different procedures foo, bar, baz and barfl if the string str contains the name of the respective procedure. Do it in a way natural to the language.


*switch语句*

```go
package main

import (
	"fmt"
)

func main() {
	str := "baz"

	switch str {
	case "foo":
		foo()
	case "bar":
		bar()
	case "baz":
		baz()
	case "barfl":
		barfl()
	}
}

func foo() {
	fmt.Println("Called foo")
}

func bar() {
	fmt.Println("Called bar")
}

func baz() {
	fmt.Println("Called baz")
}

func barfl() {
	fmt.Println("Called barfl")
}

```

`Called baz`

---

```rust
fn main() {
    fn foo() {}
    fn bar() {}
    fn baz() {}
    fn barfl() {}
    let str_ = "x";
    match str_ {
        "foo" => foo(),
        "bar" => bar(),
        "baz" => baz(),
        "barfl" => barfl(),
        _ => {}
    }
}
```

<br>

### 207. <font color="0c0a3e">Allocate a list that is automatically deallocated</font>

> Allocate a list a containing n elements (n assumed to be too large for a stack) that is automatically deallocated when the program exits the scope it is declared in.



*分配一个自动解除分配的列表*

```go
package main

import (
	"fmt"
)

type T byte

func main() {
	n := 10_000_000
	a := make([]T, n)
	fmt.Println(len(a))
}

```


*Elements have type T.
a is garbage-collected after the program exits its scope, unless we let it "escape" by taking its reference.
The runtime decides if a lives in the stack on in the heap.*

`10000000`


---

```rust
let a = vec![0; n];
```

`Heap allocations are deallocated when the variable goes out of scope.`

<br>


### 208. <font color="0c0a3e">Formula with arrays</font>


> Given the arrays a,b,c,d of equal length and the scalar e, calculate a = e*(a+b*c+cos(d)).
Store the results in a.


*对数组元素进行运算*

```go
package main

import (
	"fmt"
	"math"
)

func applyFormula(a, b, c, d []float64, e float64) {
	for i, v := range a {
		a[i] = e * (v + b[i] + c[i] + math.Cos(d[i]))
	}
}

func main() {
	a := []float64{1, 2, 3, 4}
	b := []float64{5.5, 6.6, 7.7, 8.8}
	c := []float64{9, 10, 11, 12}
	d := []float64{13, 14, 15, 16}
	e := 42.0

	fmt.Println("a is    ", a)
	applyFormula(a, b, c, d, e)
	fmt.Println("a is now", a)
}

```

```go
a is     [1 2 3 4]
a is now [689.1127648209083 786.9429631647291 879.4931076599294 1001.3783018264178]
```


---

```rust

fn main() {
    let mut a: [f32; 5] = [5., 2., 8., 9., 0.5]; // we want it to be mutable
    let b: [f32; 5] = [7., 9., 8., 0.965, 0.98]; 
    let c: [f32; 5] = [0., 0.8, 789456., 123456., 0.0003]; 
    let d: [f32; 5] = [332., 0.1, 8., 9874., 0.3]; 

    const e: f32 = 85.;

    for i in 0..a.len() {
        a[i] = e * (a[i] + b[i] * c[i] + d[i].cos());
    }

    println!("{:?}", a); //Don't have any idea about the output
}
```

`[470.29297, 866.57544, 536830750.0, 10127158.0, 123.7286]`

<br>

### 209. <font color="0c0a3e">Type with automatic deep deallocation</font>

> Declare a type t which contains a string s and an integer array n with variable size, and allocate a variable v of type t. Allocate v.s and v.n and set them to the values "Hello, world!" for s and [1,4,9,16,25], respectively. Deallocate v, automatically deallocating v.s and v.n (no memory leaks).


*自动深度解除分配的类型*

```go
package main

func main() {
	f()
}

func f() {
	type t struct {
		s string
		n []int
	}

	v := t{
		s: "Hello, world!",
		n: []int{1, 4, 9, 16, 25},
	}

	// Pretend to use v (otherwise this is a compile error)
	_ = v

	// When f exits, v and all its fields are garbage-collected, recursively
}

```

*After v goes out of scope, v and all its fields will be garbage-collected, recursively*

---

```rust
struct T {
	s: String,
	n: Vec<usize>,
}

fn main() {
	let v = T {
		s: "Hello, world!".into(),
		n: vec![1,4,9,16,25]
	};
}
```

*When a variable goes out of scope, all member variables are deallocated recursively.* 



<br>

### 211. <font color="0c0a3e">Create folder</font>

> Create the folder at path on the filesystem


*创建文件夹*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	path := "foo"
	_, err := os.Stat(path)
	b := !os.IsNotExist(err)
	fmt.Println(path, "exists:", b)

	err = os.Mkdir(path, os.ModeDir)
	if err != nil {
		panic(err)
	}

	info, err2 := os.Stat(path)
	b = !os.IsNotExist(err2)
	fmt.Println(path, "exists:", b)
	fmt.Println(path, "is a directory:", info.IsDir())
}

```

```go
foo exists: false
foo exists: true
foo is a directory: true
```


or


```go
package main

import (
	"fmt"
	"os"
)

func main() {
	path := "foo/bar"
	_, err := os.Stat(path)
	b := !os.IsNotExist(err)
	fmt.Println(path, "exists:", b)

	err = os.Mkdir(path, os.ModeDir)
	if err != nil {
		fmt.Println("Could not create", path, "with os.Mkdir")
	}

	info, err2 := os.Stat(path)
	b = !os.IsNotExist(err2)
	fmt.Println(path, "exists:", b)

	err = os.MkdirAll(path, os.ModeDir)
	if err != nil {
		fmt.Println("Could not create", path, "with os.MkdirAll")
	}

	info, err2 = os.Stat(path)
	b = !os.IsNotExist(err2)
	fmt.Println(path, "exists:", b)
	fmt.Println(path, "is a directory:", info.IsDir())
}
```

```go
foo/bar exists: false
Could not create foo/bar with os.Mkdir
foo/bar exists: false
foo/bar exists: true
foo/bar is a directory: true
```



---

```rust
use std::fs;
use std::path::Path;

fn main() {
    let path = "/tmp/goofy";
    let b: bool = Path::new(path).is_dir();
    println!("{} exists: {}", path, b);

    let r = fs::create_dir(path);
    match r {
        Err(e) => {
            println!("error creating {}: {}", path, e);
            std::process::exit(1);
        }
        Ok(_) => println!("created {}: OK", path),
    }

    let b: bool = Path::new(path).is_dir();
    println!("{} exists: {}", path, b);
}

```

```rust
/tmp/goofy exists: false
created /tmp/goofy: OK
/tmp/goofy exists: true
```

or

```rust
use std::fs;
use std::path::Path;

fn main() {
    let path = "/tmp/friends/goofy";
    let b: bool = Path::new(path).is_dir();
    println!("{} exists: {}", path, b);

    // fs::create_dir can't create parent folders
    let r = fs::create_dir(path);
    match r {
        Err(e) => println!("error creating {}: {}", path, e),
        Ok(_) => println!("created {}: OK", path),
    }

    let b: bool = Path::new(path).is_dir();
    println!("{} exists: {}", path, b);

    // fs::create_dir_all does create parent folders
    let r = fs::create_dir_all(path);
    match r {
        Err(e) => println!("error creating {}: {}", path, e),
        Ok(_) => println!("created {}: OK", path),
    }

    let b: bool = Path::new(path).is_dir();
    println!("{} exists: {}", path, b);
}
```

```rust
/tmp/friends/goofy exists: false
error creating /tmp/friends/goofy: No such file or directory (os error 2)
/tmp/friends/goofy exists: false
created /tmp/friends/goofy: OK
/tmp/friends/goofy exists: true
```

<br>

### 212. <font color="0c0a3e">Check if folder exists</font>

> Set boolean b to true if path exists on the filesystem and is a directory; false otherwise.


*检查文件夹是否存在*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	path := "foo"
	info, err := os.Stat(path)
	b := !os.IsNotExist(err) && info.IsDir()
	fmt.Println(path, "is a directory:", b)

	err = os.Mkdir(path, os.ModeDir)
	if err != nil {
		panic(err)
	}

	info, err = os.Stat(path)
	b = !os.IsNotExist(err) && info.IsDir()
	fmt.Println(path, "is a directory:", b)
}

```

```go
foo is a directory: false
foo is a directory: true
```


---

```rust
use std::path::Path;

fn main() {
    let path = "/etc";
    let b: bool = Path::new(path).is_dir();
    println!("{}: {}", path, b);

    let path = "/goofy";
    let b: bool = Path::new(path).is_dir();
    println!("{}: {}", path, b);
}

```

```rust
/etc: true
/goofy: false
```



<br>

### 215. <font color="0c0a3e">Pad string on the left</font>

> Prepend extra character c at the beginning of string s to make sure its length is at least m.
The length is the number of characters, not the number of bytes.

*左侧补齐字符串*

```go
package main

import (
	"fmt"
	"strings"
	"unicode/utf8"
)

func main() {
	m := 3
	c := "-"
	for _, s := range []string{
		"",
		"a",
		"ab",
		"abc",
		"abcd",
		"é",
	} {
		if n := utf8.RuneCountInString(s); n < m {
			s = strings.Repeat(c, m-n) + s
		}
		fmt.Println(s)
	}
}

```

```go
---
--a
-ab
abc
abcd
--é
```

---

```rust
use unicode_width::{UnicodeWidthChar, UnicodeWidthStr};
if let Some(columns_short) = m.checked_sub(s.width()) {
    let padding_width = c
        .width()
        .filter(|n| *n > 0)
        .expect("padding character should be visible");
    // Saturate the columns_short
    let padding_needed = columns_short + padding_width - 1 / padding_width;
    let mut t = String::with_capacity(s.len() + padding_needed);
    t.extend((0..padding_needed).map(|_| c)
    t.push_str(&s);
    s = t;
}
```

*This uses the Unicode display width to determine the padding needed. This will be appropriate for most uses of monospaced text.

It assumes that m won't combine with other characters to form a grapheme.*

<br>



### 217. <font color="0c0a3e">Create a Zip archive</font>


> Create a zip-file with filename name and add the files listed in list to that zip-file.


*创建压缩文件*

```go
package main

import (
	"archive/zip"
	"bytes"
	"io"
	"io/ioutil"
	"log"
	"os"
)

func main() {
	list := []string{
		"readme.txt",
		"gopher.txt",
		"todo.txt",
	}
	name := "archive.zip"

	err := makeZip(list, name)
	if err != nil {
		log.Fatal(err)
	}
}

func makeZip(list []string, name string) error {
	// Create a buffer to write our archive to.
	buf := new(bytes.Buffer)

	// Create a new zip archive.
	w := zip.NewWriter(buf)

	// Add some files to the archive.
	for _, filename := range list {
		// Open file for reading
		input, err := os.Open(filename)
		if err != nil {
			return err
		}
		// Create ZIP entry for writing
		output, err := w.Create(filename)
		if err != nil {
			return err
		}

		_, err = io.Copy(output, input)
		if err != nil {
			return err
		}
	}

	// Make sure to check the error on Close.
	err := w.Close()
	if err != nil {
		return err
	}

	N := buf.Len()
	err = ioutil.WriteFile(name, buf.Bytes(), 0777)
	if err != nil {
		return err
	}
	log.Println("Written a ZIP file of", N, "bytes")

	return nil
}

func init() {
	// Create some files in the filesystem.
	var files = []struct {
		Name, Body string
	}{
		{"readme.txt", "This archive contains some text files."},
		{"gopher.txt", "Gopher names:\nGeorge\nGeoffrey\nGonzo"},
		{"todo.txt", "Get animal handling licence.\nWrite more examples."},
	}
	for _, file := range files {
		err := ioutil.WriteFile(file.Name, []byte(file.Body), 0777)
		if err != nil {
			log.Fatal(err)
		}
	}
}

```

*list contains filenames of files existing in the filesystem.
In this example, the zip data is buffered in memory before writing to the filesystem.*

`2009/11/10 23:00:00 Written a ZIP file of 492 bytes`

---

```rust
use zip::write::FileOptions;
let path = std::path::Path::new(_name);
let file = std::fs::File::create(&path).unwrap();
let mut zip = zip::ZipWriter::new(file); zip.start_file("readme.txt", FileOptions::default())?;                                                          
zip.write_all(b"Hello, World!\n")?;
zip.finish()?;
```

or

```rust
use zip::write::FileOptions;
fn zip(_name: &str, _list: Vec<&str>) -> zip::result::ZipResult<()>
{
    let path = std::path::Path::new(_name);
    let file = std::fs::File::create(&path).unwrap();
    let mut zip = zip::ZipWriter::new(file);
    for i in _list.iter() {
        zip.start_file(i as &str, FileOptions::default())?;
    }
    zip.finish()?;
    Ok(())
}
```

<br>

### 218. <font color="0c0a3e">List intersection</font>


> Create list c containing all unique elements that are contained in both lists a and b.
c should not contain any duplicates, even if a and b do.
The order of c doesn't matter.


*列表的交集*

```go
package main

import (
	"fmt"
)

type T int

func main() {
	a := []T{4, 5, 6, 7, 8, 9, 10}
	b := []T{1, 3, 9, 5, 7, 9, 7, 7}

	// Convert to sets
	seta := make(map[T]bool, len(a))
	for _, x := range a {
		seta[x] = true
	}
	setb := make(map[T]bool, len(a))
	for _, y := range b {
		setb[y] = true
	}

	// Iterate in one pass
	var c []T
	for x := range seta {
		if setb[x] {
			c = append(c, x)
		}
	}

	fmt.Println(c)
}
```

`[5 7 9]`

---


```rust
use std::collections::HashSet;

fn main() {
    let a = vec![1, 2, 3, 4];
    let b = vec![2, 4, 6, 8, 2, 2];

    let unique_a = a.iter().collect::<HashSet<_>>();
    let unique_b = b.iter().collect::<HashSet<_>>();
    let c = unique_a.intersection(&unique_b).collect::<Vec<_>>();

    println!("c: {:?}", c);
}
```

`c: [2, 4]`

or

```rust
use std::collections::HashSet;

fn main() {
    let a = vec![1, 2, 3, 4];
    let b = vec![2, 4, 6, 8, 2, 2];

    let set_a: HashSet<_> = a.into_iter().collect();
    let set_b: HashSet<_> = b.into_iter().collect();
    let c = set_a.intersection(&set_b);

    println!("c: {:?}", c);
}
```

`c: [2, 4]`

<br>

### 219. <font color="0c0a3e">Replace multiple spaces with single space</font>


> Create string t from the value of string s with each sequence of spaces replaced by a single space.<br>Explain if only the space characters will be replaced, or the other whitespaces as well: tabs, newlines.


*用单个空格替换多个空格*

```go
package main

import (
	"fmt"
	"regexp"
)

// regexp created only once, and then reused
var whitespaces = regexp.MustCompile(`\s+`)

func main() {
	s := `
	one   two
	   three
	`

	t := whitespaces.ReplaceAllString(s, " ")

	fmt.Printf("t=%q", t)
}

```

`t=" one two three "`

---

```rust
use regex::Regex;

fn main() {
    let s = "
	one   two
	   three
	";
    let re = Regex::new(r"\s+").unwrap();
    let t = re.replace_all(s, " ");
    println!("{}", t);
}
```


` one two three `

<br>

### 220. <font color="0c0a3e">Create a tuple value</font>

> Create t consisting of 3 values having different types.<br>Explain if the elements of t are strongly typed or not.


*创建元组值a*

```go
t := []interface{}{
	2.5,
	"hello",
	make(chan int),
}
```

**A slice of empty interface may hold any values (not strongly typed).**

---

```rust
fn main() {
    let mut t = (2.5, "hello", -1);
    
    t.2 -= 4;
    println!("{:?}", t);
}
```

`(2.5, "hello", -5)`

<br>

