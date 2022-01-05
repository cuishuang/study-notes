---
title: 'Rust vs Go:常用语法对比(9)'
date: 2021-09-10 21:43:58
tags: Rust
---


<br>

### 161. <font color="0c0a3e">Multiply all the elements of a list</font>

> Multiply all the elements of the list elements by a constant c


*将list中的每个元素都乘以一个数*

```go
package main

import (
	"fmt"
)

func main() {
	const c = 5.5
	elements := []float64{2, 4, 9, 30}
	fmt.Println(elements)

	for i := range elements {
		elements[i] *= c
	}
	fmt.Println(elements)
}

```

```go
[2 4 9 30]
[11 22 49.5 165]
```

---

```rust
fn main() {
    let elements: Vec<f32> = vec![2.0, 3.5, 4.0];
    let c = 2.0;

    let elements = elements.into_iter().map(|x| c * x).collect::<Vec<_>>();

    println!("{:?}", elements);
}

```

`[4.0, 7.0, 8.0]`

<br>


### 162. <font color="0c0a3e">Execute procedures depending on options</font>

> execute bat if b is a program option and fox if f is a program option.


*根据选项执行程序*

```go
package main

import (
	"flag"
	"fmt"
	"os"
)

func init() {
	// Just for testing in the Playground, let's simulate
	// the user called this program with command line
	// flags -f and -b
	os.Args = []string{"program", "-f", "-b"}
}

var b = flag.Bool("b", false, "Do bat")
var f = flag.Bool("f", false, "Do fox")

func main() {
	flag.Parse()
	if *b {
		bar()
	}
	if *f {
		fox()
	}
	fmt.Println("The end.")
}

func bar() {
	fmt.Println("BAR")
}

func fox() {
	fmt.Println("FOX")
}

```

```go
BAR
FOX
The end.
```


---

```rust
if let Some(arg) = ::std::env::args().nth(1) {
    if &arg == "f" {
        fox();
    } else if &arg = "b" {
        bat();
    } else {
	eprintln!("invalid argument: {}", arg),
    }
} else {
    eprintln!("missing argument");
}
```

or

```rust
if let Some(arg) = ::std::env::args().nth(1) {
    match arg.as_str() {
        "f" => fox(),
        "b" => box(),
        _ => eprintln!("invalid argument: {}", arg),
    };
} else {
    eprintln!("missing argument");
}
```

<br>

### 163. <font color="0c0a3e">Print list elements by group of 2</font>

> Print all the list elements, two by two, assuming list length is even.

*两个一组打印数组元素*

```go
package main

import (
	"fmt"
)

func main() {
	list := []string{"a", "b", "c", "d", "e", "f"}

	for i := 0; i+1 < len(list); i += 2 {
		fmt.Println(list[i], list[i+1])
	}
}

```

```go
a b
c d
e f
```

---

```rust
fn main() {
    let list = [1,2,3,4,5,6];
    for pair in list.chunks(2) {
        println!("({}, {})", pair[0], pair[1]);
    }
}

```

```rust
(1, 2)
(3, 4)
(5, 6)
```

<br>

### 164. <font color="0c0a3e">Open URL in default browser</font>


> Open the URL s in the default browser.
Set boolean b to indicate whether the operation was successful.

*在默认浏览器中打开链接*

```go
import "github.com/skratchdot/open-golang/open"
b := open.Start(s) == nil
```

or

```go
func openbrowser(url string) {
	var err error

	switch runtime.GOOS {
	case "linux":
		err = exec.Command("xdg-open", url).Start()
	case "windows":
		err = exec.Command("rundll32", "url.dll,FileProtocolHandler", url).Start()
	case "darwin":
		err = exec.Command("open", url).Start()
	default:
		err = fmt.Errorf("unsupported platform")
	}
	if err != nil {
		log.Fatal(err)
	}

}
```

---

```rust
use webbrowser;
webbrowser::open(s).expect("failed to open URL");
```

<br>

### 165. <font color="0c0a3e">Last element of list</font>

> Assign to variable x the last element of list items.

*列表中的最后一个元素*

```go
package main

import (
	"fmt"
)

func main() {
	items := []string{ "what", "a", "mess" }
	
	x := items[len(items)-1]

	fmt.Println(x)
}

```

`mess`


---

```rust
fn main() {
    let items = vec![5, 6, 8, -20, 9, 42];
    let x = items[items.len()-1];
    println!("{:?}", x);
}

```

`42`

or

```rust
fn main() {
    let items = [5, 6, 8, -20, 9, 42];
    let x = items.last().unwrap();
    println!("{:?}", x);
}
```

`42`



<br>

### 166. <font color="0c0a3e">Concatenate two lists</font>


> Create list ab containing all the elements of list a, followed by all elements of list b.



*连接两个列表*

```go
package main

import (
	"fmt"
)

func main() {
	a := []string{"The ", "quick "}
	b := []string{"brown ", "fox "}

	ab := append(a, b...)

	fmt.Printf("%q", ab)
}

```

`["The " "quick " "brown " "fox "]`

or

```go
package main

import (
	"fmt"
)

func main() {
	type T string

	a := []T{"The ", "quick "}
	b := []T{"brown ", "fox "}

	var ab []T
	ab = append(append(ab, a...), b...)

	fmt.Printf("%q", ab)
}

```

`["The " "quick " "brown " "fox "]`

or

```go
package main

import (
	"fmt"
)

func main() {
	type T string

	a := []T{"The ", "quick "}
	b := []T{"brown ", "fox "}

	ab := make([]T, len(a)+len(b))
	copy(ab, a)
	copy(ab[len(a):], b)

	fmt.Printf("%q", ab)
}
```

`["The " "quick " "brown " "fox "]`

---

```rust
fn main() {
    let a = vec![1, 2];
    let b = vec![3, 4];
    let ab = [a, b].concat();
    println!("{:?}", ab);
}
```

`[1, 2, 3, 4]`

<br>

### 167. <font color="0c0a3e">Trim prefix</font>

> Create string t consisting of string s with its prefix p removed (if s starts with p).



*移除前缀*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "café-society"
	p := "café"

	t := strings.TrimPrefix(s, p)

	fmt.Println(t)
}

```

`-society`

---

```rust
fn main() {
    {
        let s = "pre_thing";
        let p = "pre_";
        let t = s.trim_start_matches(p);
        println!("{}", t);
    }
    {
        // Warning: trim_start_matches removes several leading occurrences of p, if present.
        let s = "pre_pre_thing";
        let p = "pre_";
        let t = s.trim_start_matches(p);
        println!("{}", t);
    }
}
```

```rust
thing
thing
```

or

```rust
fn main() {
    let s = "pre_pre_thing";
    let p = "pre_";

    let t = if s.starts_with(p) { &s[p.len()..] } else { s };
    println!("{}", t);
}
```

`pre_thing`


or

```rust
fn main() {
    {
        let s = "pre_thing";
        let p = "pre_";
        let t = s.strip_prefix(p).unwrap_or_else(|| s);
        println!("{}", t);
    }
    {
        // If prefix p is repeated in s, it is removed only once by strip_prefix
        let s = "pre_pre_thing";
        let p = "pre_";
        let t = s.strip_prefix(p).unwrap_or_else(|| s);
        println!("{}", t);
    }
}

```

```rust
thing
pre_thing
```




<br>


### 168. <font color="0c0a3e">Trim suffix</font>


> Create string t consisting of string s with its suffix w removed (if s ends with w).



*移除后缀*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "café-society"
	w := "society"

	t := strings.TrimSuffix(s, w)

	fmt.Println(t)
}

```

`café-`

---

```rust
fn main() {
    let s = "thing_suf";
    let w = "_suf";
    let t = s.trim_end_matches(w);
    println!("{}", t);

    let s = "thing";
    let w = "_suf";
    let t = s.trim_end_matches(w); // s does not end with w, it is left intact
    println!("{}", t);

    let s = "thing_suf_suf";
    let w = "_suf";
    let t = s.trim_end_matches(w); // removes several occurrences of w
    println!("{}", t);
}

```

```rust
thing
thing
thing
```

or

```rust
fn main() {
    let s = "thing_suf";
    let w = "_suf";
    let t = s.strip_suffix(w).unwrap_or(s);
    println!("{}", t);

    let s = "thing";
    let w = "_suf";
    let t = s.strip_suffix(w).unwrap_or(s); // s does not end with w, it is left intact
    println!("{}", t);

    let s = "thing_suf_suf";
    let w = "_suf";
    let t = s.strip_suffix(w).unwrap_or(s); // only 1 occurrence of w is removed
    println!("{}", t);
}

```


```rust
thing
thing
thing_suf
```



<br>

### 169. <font color="0c0a3e">String length</font>


> 	Assign to integer n the number of characters of string s.
Make sure that multibyte characters are properly handled.
n can be different from the number of bytes of s.


*字符串长度*

```go
package main

import "fmt"
import "unicode/utf8"

func main() {
	s := "Hello, 世界"
	n := utf8.RuneCountInString(s)

	fmt.Println(n)
}

```

`9`

---

```rust
fn main() {
    let s = "世界";

    let n = s.chars().count();

    println!("{} characters", n);
}

```

`2 characters`

<br>

### 170. <font color="0c0a3e">Get map size</font>



> Set n to the number of elements stored in mymap.<br>This is not always equal to the map capacity.


*获取map的大小*

```go
package main

import "fmt"

func main() {
	mymap := map[string]int{"a": 1, "b": 2, "c": 3}
	n := len(mymap)
	fmt.Println(n)
}
```

`3`

---

```rust
use std::collections::HashMap;

fn main() {
    let mut mymap: HashMap<&str, i32> = [("one", 1), ("two", 2)].iter().cloned().collect();
    mymap.insert("three", 3);

    let n = mymap.len();

    println!("mymap has {:?} entries", n);
}
```

`mymap has 3 entries`

<br>

### 171. <font color="0c0a3e">Add an element at the end of a list</font>


> Append element x to the list s.


*在list尾部添加元素*

```go
package main

import "fmt"

func main() {
	s := []int{1, 1, 2, 3, 5, 8, 13}
	x := 21

	s = append(s, x)

	fmt.Println(s)
}

```

`[1 1 2 3 5 8 13 21]`

---

```rust
fn main() {
    let mut s = vec![1, 2, 3];
    let x = 99;

    s.push(x);

    println!("{:?}", s);
}
```

`[1, 2, 3, 99]`

<br>

### 172. <font color="0c0a3e">Insert entry in map</font>

> Insert value v for key k in map m.


*向map中写入元素*

```go
package main

import "fmt"

func main() {
	m := map[string]int{"one": 1, "two": 2}
	k := "three"
	v := 3

	m[k] = v

	fmt.Println(m)
}
```

`map[one:1 three:3 two:2]`

---

```rust
use std::collections::HashMap;

fn main() {
    let mut m: HashMap<&str, i32> = [("one", 1), ("two", 2)].iter().cloned().collect();

    let (k, v) = ("three", 3);

    m.insert(k, v);

    println!("{:?}", m);
}

```

`{"three": 3, "one": 1, "two": 2}`

<br>


### 173. <font color="0c0a3e">Format a number with grouped thousands</font>

> Number will be formatted with a comma separator between every group of thousands.



*按千位格式化数字*

```go
package main

import (
	"fmt"

	"golang.org/x/text/language"
	"golang.org/x/text/message"
)

// The Playground doesn't work with import of external packages.
// However, you may copy this source and test it on your workstation.

func main() {
	p := message.NewPrinter(language.English)
	s := p.Sprintf("%d\n", 1000)

	fmt.Println(s)
	// Output:
	// 1,000
}

```

`1,000`

or

```go
package main

import (
	"fmt"
	"github.com/floscodes/golang-thousands"
	"strconv"
)

// The Playground takes more time when importing external packages.
// However, you may want to copy this source and test it on your workstation.

func main() {
	n := strconv.Itoa(23489)
	s := thousands.Separate(n, "en")

	fmt.Println(s)
	// Output:
	// 23,489
}

```

`23,489`

---

```rust
use separator::Separatable;
println!("{}", 1000.separated_string());
```

<br>

### 174. <font color="0c0a3e">Make HTTP POST request</font>

> Make a HTTP request with method POST to URL u


*发起http POST请求*

```go
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"net"
	"net/http"
)

func main() {
	contentType := "text/plain"
	var body io.Reader
	u := "http://" + localhost + "/hello"

	response, err := http.Post(u, contentType, body)
	check(err)
	buffer, err := ioutil.ReadAll(response.Body)
	check(err)
	fmt.Println("POST response:", response.StatusCode, string(buffer))

	response, err = http.Get(u)
	check(err)
	buffer, err = ioutil.ReadAll(response.Body)
	check(err)
	fmt.Println("GET  response:", response.StatusCode, string(buffer))
}

const localhost = "127.0.0.1:3000"

func init() {
	http.HandleFunc("/hello", myHandler)
	startServer()
}

func myHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != "POST" {
		w.WriteHeader(http.StatusBadRequest)
		fmt.Fprintf(w, "Refusing request verb %q", r.Method)
		return
	}
	fmt.Fprintf(w, "Hello POST :)")
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


```
POST response: 200 Hello Alice (POST)
GET  response: 400 Refusing request verb "GET"
```



or


```go
package main

import (
	"fmt"
	"io/ioutil"
	"net"
	"net/http"
	"net/url"
)

func main() {
	formValues := url.Values{
		"who": []string{"Alice"},
	}
	u := "http://" + localhost + "/hello"

	response, err := http.PostForm(u, formValues)
	check(err)
	buffer, err := ioutil.ReadAll(response.Body)
	check(err)
	fmt.Println("POST response:", response.StatusCode, string(buffer))

	response, err = http.Get(u)
	check(err)
	buffer, err = ioutil.ReadAll(response.Body)
	check(err)
	fmt.Println("GET  response:", response.StatusCode, string(buffer))
}

const localhost = "127.0.0.1:3000"

func init() {
	http.HandleFunc("/hello", myHandler)
	startServer()
}

func myHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != "POST" {
		w.WriteHeader(http.StatusBadRequest)
		fmt.Fprintf(w, "Refusing request verb %q", r.Method)
		return
	}
	fmt.Fprintf(w, "Hello %s (POST)", r.FormValue("who"))
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






---

```rust
[dependencies]
error-chain = "0.12.4"
reqwest = { version = "0.11.2", features = ["blocking"] }

use error_chain::error_chain;
use std::io::Read;
let client = reqwest::blocking::Client::new();
let mut response = client.post(u).body("abc").send()?;
```

<br>

### 175. <font color="0c0a3e">Bytes to hex string</font>


> From array a of n bytes, build the equivalent hex string s of 2n digits.
Each byte (256 possible values) is encoded as two hexadecimal characters (16 possible values per digit).


*字节转十六进制字符串*

```go
package main

import (
	"encoding/hex"
	"fmt"
)

func main() {
	a := []byte("Hello")

	s := hex.EncodeToString(a)

	fmt.Println(s)
}
```

`48656c6c6f`

---

```rust
use core::fmt::Write;

fn main() -> core::fmt::Result {
    let a = vec![22, 4, 127, 193];
    let n = a.len();
    
    let mut s = String::with_capacity(2 * n);
    for byte in a {
        write!(s, "{:02X}", byte)?;
    }
    
    dbg!(s);
    Ok(())
}
```

`[src/main.rs:12] s = "16047FC1"`


<br>

### 176. <font color="0c0a3e">Hex string to byte array</font>


> From hex string s of 2n digits, build the equivalent array a of n bytes.
Each pair of hexadecimal characters (16 possible values per digit) is decoded into one byte (256 possible values).



*十六进制字符串转字节数组*

```go
package main

import (
	"encoding/hex"
	"fmt"
	"log"
)

func main() {
	s := "48656c6c6f"

	a, err := hex.DecodeString(s)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(a)
	fmt.Println(string(a))
}

```

```go
[72 101 108 108 111]
Hello
```


---

```rust
use hex::FromHex
let a: Vec<u8> = Vec::from_hex(s).expect("Invalid Hex String");
```


<br>

### 178. <font color="0c0a3e">Check if point is inside rectangle</font>


> 	Set boolean b to true if if the point with coordinates (x,y) is inside the rectangle with coordinates (x1,y1,x2,y2) , or to false otherwise.
Describe if the edges are considered to be inside the rectangle.

*检查点是否在矩形内*

```go
package main

import (
	"fmt"
	"image"
)

func main() {
	x1, y1, x2, y2 := 1, 1, 50, 100
	r := image.Rect(x1, y1, x2, y2)

	x, y := 10, 10
	p := image.Pt(x, y)
	b := p.In(r)
	fmt.Println(b)

	x, y = 100, 100
	p = image.Pt(x, y)
	b = p.In(r)
	fmt.Println(b)
}

```

```go
true
false
```


---


```rust
struct Rect {
    x1: i32,
    x2: i32,
    y1: i32,
    y2: i32,
}

impl Rect {
    fn contains(&self, x: i32, y: i32) -> bool {
        return self.x1 < x && x < self.x2 && self.y1 < y && y < self.y2;
    }
}
```



<br>

### 179. <font color="0c0a3e">Get center of a rectangle</font>


> Return the center c of the rectangle with coördinates(x1,y1,x2,y2)


*获取矩形的中心*

```go
import "image"
c := image.Pt((x1+x2)/2, (y1+y2)/2)
```

---

```rust
struct Rectangle {
    x1: f64,
    y1: f64,
    x2: f64,
    y2: f64,
}

impl Rectangle {
    pub fn center(&self) -> (f64, f64) {
	    ((self.x1 + self.x2) / 2.0, (self.y1 + self.y2) / 2.0)
    }
}

fn main() {
    let r = Rectangle {
        x1: 5.,
        y1: 5.,
        x2: 10.,
        y2: 10.,
    };
    
    println!("{:?}", r.center());
}
```

`(7.5, 7.5)`

<br>

### 180. <font color="0c0a3e">List files in directory</font>

> Create list x containing the contents of directory d.<br>x may contain files and subfolders.<br>No recursive subfolder listing.

*列出目录中的文件*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
)

func main() {
	d := "/"

	x, err := ioutil.ReadDir(d)
	if err != nil {
		log.Fatal(err)
	}

	for _, f := range x {
		fmt.Println(f.Name())
	}
}

```

```go
.dockerenv
bin
dev
etc
home
lib
lib64
proc
root
sys
tmp
tmpfs
usr
var
```

---

```rust
use std::fs;

fn main() {
    let d = "/etc";

    let x = fs::read_dir(d).unwrap();

    for entry in x {
        let entry = entry.unwrap();
        println!("{:?}", entry.path());
    }
}

```


or

```rust
fn main() {
    let d = "/etc";

    let x = std::fs::read_dir(d)
        .unwrap()
        .collect::<Result<Vec<_>, _>>()
        .unwrap();

    for entry in x {
        println!("{:?}", entry.path());
    }
}
```


```rust
"/etc/issue.net"
"/etc/bindresvport.blacklist"
"/etc/rc1.d"
"/etc/hostname"
"/etc/xattr.conf"
"/etc/resolv.conf"
"/etc/pam.conf"
"/etc/mke2fs.conf"
"/etc/e2scrub.conf"
"/etc/update-motd.d"
"/etc/terminfo"
"/etc/alternatives"
"/etc/ld.so.cache"
"/etc/networks"
"/etc/profile"
"/etc/debconf.conf"
"/etc/security"
"/etc/.pwd.lock"
"/etc/gai.conf"
"/etc/dpkg"
"/etc/rc3.d"
"/etc/fstab"
"/etc/gshadow"
"/etc/sysctl.conf"
"/etc/rc2.d"
"/etc/selinux"
"/etc/ld.so.conf.d"
"/etc/os-release"
"/etc/libaudit.conf"
"/etc/login.defs"
"/etc/skel"
"/etc/shells"
"/etc/rc4.d"
"/etc/cron.d"
"/etc/default"
"/etc/lsb-release"
"/etc/apt"
"/etc/debian_version"
"/etc/machine-id"
"/etc/deluser.conf"
"/etc/group"
"/etc/legal"
"/etc/rc6.d"
"/etc/init.d"
"/etc/sysctl.d"
"/etc/pam.d"
"/etc/passwd"
"/etc/rc5.d"
"/etc/bash.bashrc"
"/etc/hosts"
"/etc/rc0.d"
"/etc/environment"
"/etc/cron.daily"
"/etc/shadow"
"/etc/ld.so.conf"
"/etc/subgid"
"/etc/opt"
"/etc/logrotate.d"
"/etc/subuid"
"/etc/profile.d"
"/etc/adduser.conf"
"/etc/issue"
"/etc/rmt"
"/etc/host.conf"
"/etc/rcS.d"
"/etc/nsswitch.conf"
"/etc/systemd"
"/etc/kernel"
"/etc/mtab"
"/etc/shadow-"
"/etc/passwd-"
"/etc/subuid-"
"/etc/gshadow-"
"/etc/subgid-"
"/etc/group-"
"/etc/ethertypes"
"/etc/logcheck"
"/etc/gss"
"/etc/bash_completion.d"
"/etc/X11"
"/etc/perl"
"/etc/ca-certificates"
"/etc/protocols"
"/etc/ca-certificates.conf"
"/etc/python2.7"
"/etc/localtime"
"/etc/xdg"
"/etc/timezone"
"/etc/mailcap.order"
"/etc/emacs"
"/etc/ssh"
"/etc/magic.mime"
"/etc/services"
"/etc/ssl"
"/etc/ldap"
"/etc/rpc"
"/etc/mime.types"
"/etc/magic"
"/etc/mailcap"
"/etc/inputrc"
```



<br>

