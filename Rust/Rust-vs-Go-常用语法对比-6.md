---
title: 'Rust vs Go:常用语法对比(6)'
date: 2021-09-07 21:31:07
tags: Rust
---


<br>

### 101. <font color="b33f62">Load from HTTP GET request into a string</font>

> Make an HTTP request with method GET to URL u, then store the body of the response in string s.

*发起http请求*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net"
	"net/http"
)

func main() {
	u := "http://" + localhost + "/hello?name=Inigo+Montoya"

	res, err := http.Get(u)
	check(err)
	buffer, err := ioutil.ReadAll(res.Body)
	res.Body.Close()
	check(err)
	s := string(buffer)

	fmt.Println("GET  response:", res.StatusCode, s)
}

const localhost = "127.0.0.1:3000"

func init() {
	http.HandleFunc("/hello", myHandler)
	startServer()
}

func myHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello %s", r.FormValue("name"))
}

func startServer() {
	listener, err := net.Listen("tcp", localhost)
	check(err)

	go http.Serve(listener, nil)
}

func check(err error) {
	if err != nil {
		panic(err)
	}
}

```

<font size=2 color="orange">res has type *http.Response.
buffer has type []byte.
It is idiomatic and strongly recommended to check errors at each step.
</font>

`GET  response: 200 Hello Inigo Montoya`

---

```rust
extern crate reqwest;
use reqwest::Client;
let client = Client::new();
let s = client.get(u).send().and_then(|res| res.text())?;
```

or

```rust
[dependencies]
ureq = "1.0"
let s = ureq::get(u).call().into_string()?;
```


or

```rust
[dependencies]
error-chain = "0.12.4"
reqwest = { version = "0.11.2", features = ["blocking"] }

use error_chain::error_chain;
use std::io::Read;
let mut response = reqwest::blocking::get(u)?;
let mut s = String::new();
response.read_to_string(&mut s)?;
```


<br>

### 102. <font color="7b1e7a">Load from HTTP GET request into a file</font>

> Make an HTTP request with method GET to URL u, then store the body of the response in file result.txt. Try to save the data as it arrives if possible, without having all its content in memory at once.


*发起http请求*

```go
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"net"
	"net/http"
	"os"
)

func main() {
	err := saveGetResponse()
	check(err)
	err = readFile()
	check(err)

	fmt.Println("Done.")
}

func saveGetResponse() error {
	u := "http://" + localhost + "/hello?name=Inigo+Montoya"

	fmt.Println("Making GET request")
	resp, err := http.Get(u)
	if err != nil {
		return err
	}
	defer resp.Body.Close()
	if resp.StatusCode != 200 {
		return fmt.Errorf("Status: %v", resp.Status)
	}

	fmt.Println("Saving data to file")
	out, err := os.Create("/tmp/result.txt")
	if err != nil {
		return err
	}
	defer out.Close()
	_, err = io.Copy(out, resp.Body)
	if err != nil {
		return err
	}
	return nil
}

func readFile() error {
	fmt.Println("Reading file")
	buffer, err := ioutil.ReadFile("/tmp/result.txt")
	if err != nil {
		return err
	}
	fmt.Printf("Saved data is %q\n", string(buffer))
	return nil
}

const localhost = "127.0.0.1:3000"

func init() {
	http.HandleFunc("/hello", myHandler)
	startServer()
}

func myHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello %s", r.FormValue("name"))
}

func startServer() {
	listener, err := net.Listen("tcp", localhost)
	check(err)

	go http.Serve(listener, nil)
}

func check(err error) {
	if err != nil {
		panic(err)
	}
}
```


<font size=2 color="orange">resp has type `*`http.Response.
It is idiomatic and strongly recommended to check errors at each step, except for the calls to Close.</font>


```go
Making GET request
Saving data to file
Reading file
Saved data is "Hello Inigo Montoya"
Done.
```



---


```rust
extern crate reqwest;
use reqwest::Client;
use std::fs::File;
let client = Client::new();
match client.get(&u).send() {
    Ok(res) => {
        let file = File::create("result.txt")?;
        ::std::io::copy(res, file)?;
    },
    Err(e) => eprintln!("failed to send request: {}", e),
};
```



<br>

### 105. <font color="f3c677">Current executable name</font>

> Assign to string s the name of the currently executing program (but not its full path).


*当前可执行文件名称*

*将当前正在执行的程序的名称分配给字符串s(但不是它的完整路径)。*

```go
package main

import (
	"fmt"
	"os"
	"path/filepath"
)

func main() {
	var s string

	path := os.Args[0]
	s = filepath.Base(path)

	fmt.Println(s)
}

```

`play`

---

```rust
fn get_exec_name() -> Option<String> {
    std::env::current_exe()
        .ok()
        .and_then(|pb| pb.file_name().map(|s| s.to_os_string()))
        .and_then(|s| s.into_string().ok())
}

fn main() -> () {
    let s = get_exec_name().unwrap();
    println!("{}", s);
}
```

`playground`

or

```rust
fn main() {
    let s = std::env::current_exe()
        .expect("Can't get the exec path")
        .file_name()
        .expect("Can't get the exec name")
        .to_string_lossy()
        .into_owned();
    
    println!("{}", s);
}
```

`playground`

<br>

### 106. <font color="7bdff2">Get program working directory</font>

> Assign to string dir the path of the working directory.
(This is not necessarily the folder containing the executable itself)

*获取程序的工作路径*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	dir, err := os.Getwd()
	fmt.Println(dir, err)
}
```

`/ <nil>`

---

```rust
use std::env;

fn main() {
    let dir = env::current_dir().unwrap();

    println!("{:?}", dir);
}

```

`"/playground"`

<br>

### 107. <font color="b2f7ef">Get folder containing current program</font>

> Assign to string dir the path of the folder containing the currently running executable.
(This is not necessarily the working directory, though.)



*获取包含当前程序的文件夹*

```go
package main

import (
	"fmt"
	"os"
	"path/filepath"
)

func main() {
	var dir string

	programPath := os.Args[0]
	absolutePath, err := filepath.Abs(programPath)
	if err != nil {
		panic(err)
	}
	dir = filepath.Dir(absolutePath)

	fmt.Println(dir)
}

```

`/tmpfs`

---


```rust
let dir = std::env::current_exe()?
    .canonicalize()
    .expect("the current exe should exist")
    .parent()
    .expect("the current exe should be a file")
    .to_string_lossy()
    .to_owned();
```

*Rust doesn't represent paths as Strings, so we need to convert the Path returned from Path::parent. This code chooses to do this lossily, replacing characters it doesn't recognize with �*


<br>

### 109. <font color="f7d6e0">Number of bytes of a type</font>


> Set n to the number of bytes of a variable t (of type T).

*获取某个类型的字节数*

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var t T
	tType := reflect.TypeOf(t)
	n := tType.Size()

	fmt.Println("A", tType, "object is", n, "bytes.")
}

type Person struct {
	FirstName string
	Age       int
}

// T is a type alias, to stick to the idiom statement.
// T has the same memory footprint per value as Person.
type T Person

```

`A main.T object is 24 bytes.`

---

```rust
// T has (8 + 4) == 12 bytes of data
struct T(f64, i32);

fn main() {
    let n = ::std::mem::size_of::<T>();

    println!("{} bytes", n);
    // T has size 16, which is "the offset in bytes between successive elements in an array with item type T"
}
```

`16 bytes`


<br>

### 110. <font color="f2b5d4">Check if string is blank</font>

> Set boolean blank to true if string s is empty, or null, or contains only whitespace ; false otherwise.

*检查字符串是否空白*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	for _, s := range []string{
		"",
		"a",
		" ",
		"\t \n",
		"_",
	} {
		blank := strings.TrimSpace(s) == ""

		if blank {
			fmt.Printf("%q is blank\n", s)
		} else {
			fmt.Printf("%q is not blank\n", s)
		}
	}
}
```
```go
"" is blank
"a" is not blank
" " is blank
"\t \n" is blank
"_" is not blank
```


---

```rust
fn main() {
    let list = vec!["", " ", "  ", "\t", "\n", "a", " b "];
    for s in list {
        let blank = s.trim().is_empty();

        if blank {
            println!("{:?}\tis blank", s)
        } else {
            println!("{:?}\tis not blank", s)
        }
    }
}
```

```rust
""	is blank
" "	is blank
"  "	is blank
"\t"	is blank
"\n"	is blank
"a"	is not blank
" b "	is not blank
```

<br>

### 111. <font color="247ba0">Launch other program</font>

> From current process, run program x with command-line parameters "a", "b".


*运行其他程序*

```go
package main

import (
	"fmt"
	"os/exec"
)

func main() {
	err := exec.Command("x", "a", "b").Run()
	fmt.Println(err)
}

```



`exec: "x": executable file not found in $PATH`

---

```rust
use std::process::Command;

fn main() {
    let child = Command::new("ls")
        .args(&["/etc"])
        .spawn()
        .expect("failed to execute process");

    let output = child.wait_with_output().expect("Failed to wait on child");
    let output = String::from_utf8(output.stdout).unwrap();

    println!("{}", output);
}

```

```rust
X11
adduser.conf
alternatives
apt
bash.bashrc
bash_completion.d
bindresvport.blacklist
ca-certificates
ca-certificates.conf
cron.d
cron.daily
debconf.conf
debian_version
default
deluser.conf
dpkg
e2scrub.conf
emacs
environment
ethertypes
fstab
gai.conf
group
group-
gshadow
gshadow-
gss
host.conf
hostname
hosts
init.d
inputrc
issue
issue.net
kernel
ld.so.cache
ld.so.conf
ld.so.conf.d
ldap
legal
libaudit.conf
localtime
logcheck
login.defs
logrotate.d
lsb-release
machine-id
magic
magic.mime
mailcap
mailcap.order
mime.types
mke2fs.conf
mtab
networks
nsswitch.conf
opt
os-release
pam.conf
pam.d
passwd
passwd-
perl
profile
profile.d
protocols
python2.7
rc0.d
rc1.d
rc2.d
rc3.d
rc4.d
rc5.d
rc6.d
rcS.d
resolv.conf
rmt
rpc
security
selinux
services
shadow
shadow-
shells
skel
ssh
ssl
subgid
subgid-
subuid
subuid-
sysctl.conf
sysctl.d
systemd
terminfo
timezone
update-motd.d
xattr.conf
xdg
```

or


```rust
use std::process::Command;

fn main() {
    let output = Command::new("ls")
        .args(&["/etc"])
        .output()
        .expect("failed to execute process");

    let output = String::from_utf8(output.stdout).unwrap();

    println!("{}", output);
}

```

or

```rust
use std::process::Command;

fn main() {
    let status = Command::new("ls")
        .args(&["/etc"])
        .status()
        .expect("failed to execute process");

    // exit code is outputted after _ls_ runs
    println!("{}", status);
}

```



<br>

### 112. <font color="70c1b3">Iterate over map entries, ordered by keys</font>

> Print each key k with its value x from an associative array mymap, in ascending order of k.


*遍历map，按key排序*

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	mymap := map[string]int{
		"one":   1,
		"two":   2,
		"three": 3,
		"four":  4,
	}

	keys := make([]string, 0, len(mymap))
	for k := range mymap {
		keys = append(keys, k)
	}
	sort.Strings(keys)

	for _, k := range keys {
		x := mymap[k]
		fmt.Println("Key =", k, ", Value =", x)
	}
}

```

```go
Key = four , Value = 4
Key = one , Value = 1
Key = three , Value = 3
Key = two , Value = 2
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
    mymap.insert("five", 5);
    mymap.insert("six", 6);

    // Iterate over map entries, ordered by keys, which is NOT the numerical order
    for (k, x) in mymap {
        println!("({}, {})", k, x);
    }
}

```

```rust
(five, 5)
(four, 4)
(one, 1)
(six, 6)
(three, 3)
(two, 2)
```



<br>

### 113. <font color="b2dbbf">Iterate over map entries, ordered by values</font>


> Print each key k with its value x from an associative array mymap, in ascending order of x.
Note that multiple entries may exist for the same value x.

*遍历map，按值排序*

```go
package main

import (
	"fmt"
	"sort"
)

type entry struct {
	key   string
	value int
}
type entries []entry

func (list entries) Len() int           { return len(list) }
func (list entries) Less(i, j int) bool { return list[i].value < list[j].value }
func (list entries) Swap(i, j int)      { list[i], list[j] = list[j], list[i] }

func main() {
	mymap := map[string]int{
		"one":   1,
		"two":   2,
		"three": 3,
		"four":  4,
		"dos":   2,
		"deux":  2,
	}

	entries := make(entries, 0, len(mymap))
	for k, x := range mymap {
		entries = append(entries, entry{key: k, value: x})
	}
	sort.Sort(entries)

	for _, e := range entries {
		fmt.Println("Key =", e.key, ", Value =", e.value)
	}
}

```

```go
Key = one , Value = 1
Key = dos , Value = 2
Key = deux , Value = 2
Key = two , Value = 2
Key = three , Value = 3
Key = four , Value = 4
```

or

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	mymap := map[string]int{
		"one":   1,
		"two":   2,
		"three": 3,
		"four":  4,
		"dos":   2,
		"deux":  2,
	}

	type entry struct {
		key   string
		value int
	}

	entries := make([]entry, 0, len(mymap))
	for k, x := range mymap {
		entries = append(entries, entry{key: k, value: x})
	}
	sort.Slice(entries, func(i, j int) bool {
		return entries[i].value < entries[j].value
	})

	for _, e := range entries {
		fmt.Println("Key =", e.key, ", Value =", e.value)
	}
}

```

```go
Key = one , Value = 1
Key = two , Value = 2
Key = dos , Value = 2
Key = deux , Value = 2
Key = three , Value = 3
Key = four , Value = 4
```


---

```rust
use itertools::Itertools;
use std::collections::HashMap;

fn main() {
    let mut mymap = HashMap::new();
    mymap.insert(1, 3);
    mymap.insert(2, 6);
    mymap.insert(3, 4);
    mymap.insert(4, 1);
    
    for (k, x) in mymap.iter().sorted_by_key(|x| x.1) {
        println!("[{},{}]", k, x);
    }
}
```

```rust
[4,1]
[1,3]
[3,4]
[2,6]
```

or

```rust
use std::collections::HashMap;

fn main() {
    let mut mymap = HashMap::new();
    mymap.insert(1, 3);
    mymap.insert(2, 6);
    mymap.insert(3, 4);
    mymap.insert(4, 1);
    
    let mut items: Vec<_> = mymap.iter().collect();
    items.sort_by_key(|item| item.1);
    for (k, x) in items {
        println!("[{},{}]", k, x);
    }
}
```

```rust
[4,1]
[1,3]
[3,4]
[2,6]
```

<br>

### 114. <font color="f3ffbd">Test deep equality</font>


> Set boolean b to true if objects x and y contain the same values, recursively comparing all referenced elements in x and y.
Tell if the code correctly handles recursive types.

*深度判等*

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	x := Foo{9, "Hello", []bool{false, true}, map[int]float64{1: 1.0, 2: 2.0}, &Bar{"Babar"}}

	list := []Foo{
		{9, "Bye", []bool{false, true}, map[int]float64{1: 1.0, 2: 2.0}, &Bar{"Babar"}},
		{9, "Hello", []bool{false, false}, map[int]float64{1: 1.0, 2: 2.0}, &Bar{"Babar"}},
		{9, "Hello", []bool{false, true}, map[int]float64{1: 3.0, 2: 2.0}, &Bar{"Babar"}},
		{9, "Hello", []bool{false, true}, map[int]float64{1: 1.0, 5: 2.0}, &Bar{"Babar"}},
		{9, "Hello", []bool{false, true}, map[int]float64{1: 1.0, 2: 2.0}, &Bar{"Batman"}},
		{9, "Hello", []bool{false, true}, map[int]float64{1: 1.0, 2: 2.0}, &Bar{"Babar"}},
	}

	for i, y := range list {
		b := reflect.DeepEqual(x, y)
		if b {
			fmt.Println("x deep equals list[", i, "]")

		} else {
			fmt.Println("x doesn't deep equal list[", i, "]")
		}
	}
}

type Foo struct {
	int
	str     string
	bools   []bool
	mapping map[int]float64
	bar     *Bar
}

type Bar struct {
	name string
}

```

```go
x doesn't deep equal list[ 0 ]
x doesn't deep equal list[ 1 ]
x doesn't deep equal list[ 2 ]
x doesn't deep equal list[ 3 ]
x doesn't deep equal list[ 4 ]
x deep equals list[ 5 ]
```


---

```rust
let b = x == y;
```

*The == operator can only be used by having a type implement PartialEq.*

<br>

### 115. <font color="ff1654">Compare dates</font>


> Set boolean b to true if date d1 is strictly before date d2 ; false otherwise.


*日期比较*

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	d1 := time.Now()
	d2 := time.Date(2020, time.November, 10, 23, 0, 0, 0, time.UTC)

	b := d1.Before(d2)

	fmt.Println(b)
}
```

`true`

---

```rust
extern crate chrono;
use chrono::prelude::*;
let b = d1 < d2;
```

[doc -- Struct chrono::Date](https://docs.rs/chrono/0.4.3/chrono/struct.Date.html)

<br>

### 116. <font color="565264">Remove occurrences of word from string</font>

> Remove all occurrences of string w from string s1, and store the result in s2.


*去除指定字符串*

```go
package main

import "fmt"
import "strings"

func main() {
	var s1 = "foobar"
	var w = "foo"
	s2 := strings.Replace(s1, w, "", -1)
	fmt.Println(s2)
}
```

`bar`

---


```rust
fn main() {
    let s1 = "foobar";
    let w = "foo";
    let s2 = s1.replace(w, "");
    println!("{}", s2);
}
```

`bar`

or

```rust
fn main() {
    let s1 = "foobar";
    let w = "foo";

    let s2 = str::replace(s1, w, "");

    println!("{}", s2);
}
```

`bar`


<br>

### 117. <font color="706677">Get list size</font>

*获取list的大小*

```go
package main

import "fmt"

func main() {
	// x is a slice
	x := []string{"a", "b", "c"}
	n := len(x)
	fmt.Println(n)

	// y is an array
	y := [4]string{"a", "b", "c"}
	n = len(y)
	fmt.Println(n)
}
```

```go
3
4
```

---

```rust
fn main() {
    let x = vec![11, 22, 33];

    let n = x.len();

    println!("x has {} elements", n);
}
```

`x has 3 elements`

<br>

### 118. <font color="a6808c">List to set</font>


> Create set y from list x.
x may contain duplicates. y is unordered and has no repeated values.


*从list到set*



```go
package main

import "fmt"

func main() {
	x := []string{"b", "a", "b", "c"}
	fmt.Println("x =", x)

	y := make(map[string]struct{}, len(x))
	for _, v := range x {
		y[v] = struct{}{}
	}

	fmt.Println("y =", y)
}
```

```go
x = [b a b c]
y = map[a:{} b:{} c:{}]
```


---



```rust
use std::collections::HashSet;

fn main() {
    let x: Vec<i32> = vec![1, 7, 3, 1];
    println!("x: {:?}", x);

    let y: HashSet<_> = x.into_iter().collect();
    println!("y: {:?}", y);
}
```

```rust
x: [1, 7, 3, 1]
y: {1, 7, 3}
```

<br>

### 119. <font color="ccb7ae"> Deduplicate list</font>


> Remove duplicates from list x.
Explain if original order is preserved.




*list去重*

```go
package main

import "fmt"

func main() {
	type T string
	x := []T{"b", "a", "b", "c"}
	fmt.Println("x =", x)

	y := make(map[T]struct{}, len(x))
	for _, v := range x {
		y[v] = struct{}{}
	}
	x2 := make([]T, 0, len(y))
	for _, v := range x {
		if _, ok := y[v]; ok {
			x2 = append(x2, v)
			delete(y, v)
		}
	}
	x = x2

	fmt.Println("x =", x)
}

```

```go
x = [b a b c]
x = [b a c]
```

or

```go
package main

import "fmt"

func main() {
	type T string
	x := []T{"b", "a", "b", "b", "c", "b", "a"}
	fmt.Println("x =", x)

	seen := make(map[T]bool)
	j := 0
	for _, v := range x {
		if !seen[v] {
			x[j] = v
			j++
			seen[v] = true
		}
	}
	x = x[:j]

	fmt.Println("x =", x)
}
```

```go
x = [b a b b c b a]
x = [b a c]
```

or


```go
package main

import "fmt"

type T *int64

func main() {
	var a, b, c, d int64 = 11, 22, 33, 11
	x := []T{&b, &a, &b, &b, &c, &b, &a, &d}
	print(x)

	seen := make(map[T]bool)
	j := 0
	for _, v := range x {
		if !seen[v] {
			x[j] = v
			j++
			seen[v] = true
		}
	}
	for i := j; i < len(x); i++ {
		// Avoid memory leak
		x[i] = nil
	}
	x = x[:j]

	// Now x has only distinct pointers (even if some point to int64 values that are the same)
	print(x)
}

func print(a []T) {
	glue := ""
	for _, p := range a {
		fmt.Printf("%s%d", glue, *p)
		glue = ", "
	}
	fmt.Println()
}
```

```go
22, 11, 22, 22, 33, 22, 11, 11
22, 11, 33, 11
```

---

```rust
fn main() {
    let mut x = vec![1, 2, 3, 4, 3, 2, 2, 2, 2, 2, 2];
    x.sort();
    x.dedup();

    println!("{:?}", x);
}
```

`[1, 2, 3, 4]`


or

```rust
use itertools::Itertools;

fn main() {
    let x = vec![1, 2, 3, 4, 3, 2, 2, 2, 2, 2, 2];
    let dedup: Vec<_> = x.iter().unique().collect();

    println!("{:?}", dedup);
}
```

`[1, 2, 3, 4]`

<br>

### 120. <font color="d6cfcb">Read integer from stdin</font>

> Read an integer value from the standard input into variable n


*从标准输入中读取整数*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

// This string simulates the keyboard entry.
var userInput string = `42 017`

func main() {
	var i int
	_, err := fmt.Scan(&i)
	fmt.Println(i, err)

	// The second value starts with 0, thus is interpreted as octal!
	var j int
	_, err = fmt.Scan(&j)
	fmt.Println(j, err)
}

// The Go Playground doesn't actually read os.Stdin, so this
// workaround writes some data on virtual FS in a file, and then
// sets this file as the new Stdin.
//
// Note that the init func is run before main.
func init() {
	err := ioutil.WriteFile("/tmp/stdin", []byte(userInput), 0644)
	if err != nil {
		panic(err)
	}
	fileIn, err := os.Open("/tmp/stdin")
	if err != nil {
		panic(err)
	}
	os.Stdin = fileIn
}

```

```go
42 <nil>
15 <nil>
```

or


```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

// This string simulates the keyboard entry.
var userInput string = `42 017`

func main() {
	var i int
	_, err := fmt.Scanf("%d", &i)
	fmt.Println(i, err)

	var j int
	_, err = fmt.Scanf("%d", &j)
	fmt.Println(j, err)
}

// The Go Playground doesn't actually read os.Stdin, so this
// workaround writes some data on virtual FS in a file, and then
// sets this file as the new Stdin.
//
// Note that the init func is run before main.
func init() {
	err := ioutil.WriteFile("/tmp/stdin", []byte(userInput), 0644)
	if err != nil {
		panic(err)
	}
	fileIn, err := os.Open("/tmp/stdin")
	if err != nil {
		panic(err)
	}
	os.Stdin = fileIn
}
```

```go
42 <nil>
17 <nil>
```

---


```rust
fn get_input() -> String {
    let mut buffer = String::new();
    std::io::stdin().read_line(&mut buffer).expect("Failed");
    buffer
}

let n = get_input().trim().parse::<i64>().unwrap();
```

or

```rust
use std::io;
let mut input = String::new();
io::stdin().read_line(&mut input).unwrap();
let n: i32 = input.trim().parse().unwrap();s

```

or

```rust
use std::io::BufRead;
let n: i32 = std::io::stdin()
    .lock()
    .lines()
    .next()
    .expect("stdin should be available")
    .expect("couldn't read from stdin")
    .trim()
    .parse()
    .expect("input was not an integer");
```

<br>


