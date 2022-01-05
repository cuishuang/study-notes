---
title: 'Rust vs Go:常用语法对比(10)'
date: 2021-09-11 22:43:58
tags: Rust
---


<br>



### 182. <font color="0c0a3e">Quine program</font>

> 	Output the source of the program.

*输出程序的源代码*

```go
package main

import "fmt"

func main() {
	fmt.Printf("%s%c%s%c\n", s, 0x60, s, 0x60)
}

var s = `package main

import "fmt"

func main() {
	fmt.Printf("%s%c%s%c\n", s, 0x60, s, 0x60)
}

var s = `

```

输出：

```go
package main

import "fmt"

func main() {
	fmt.Printf("%s%c%s%c\n", s, 0x60, s, 0x60)
}

var s = `package main

import "fmt"

func main() {
	fmt.Printf("%s%c%s%c\n", s, 0x60, s, 0x60)
}

var s = `
```



<br>


另一种写法：

[//go:embed 入门](https://taoshu.in/go/how-to-use-go-embed.html)

Quine 是一种可以输出自身源码的程序。利用 go:embed 我们可以轻松实现 quine 程序：


```go
package main

import (
    _ "embed"
    "fmt"
)

//go:embed quine.go
var src string

func main() {
    fmt.Print(src)
}
```



---

```rust
fn main() {
    let x = "fn main() {\n    let x = ";
    let y = "print!(\"{}{:?};\n    let y = {:?};\n    {}\", x, x, y, y)\n}\n";
    print!("{}{:?};
    let y = {:?};
    {}", x, x, y, y)
}
```


输出：

```rust
fn main() {
    let x = "fn main() {\n    let x = ";
    let y = "print!(\"{}{:?};\n    let y = {:?};\n    {}\", x, x, y, y)\n}\n";
    print!("{}{:?};
    let y = {:?};
    {}", x, x, y, y)
}
```

or


```rust
fn main(){print!("{},{0:?})}}","fn main(){print!(\"{},{0:?})}}\"")}
```

输出：

`fn main(){print!("{},{0:?})}}","fn main(){print!(\"{},{0:?})}}\"")}`


<br>

### 184. <font color="0c0a3e">Tomorrow</font>


> Assign to variable t a string representing the day, month and year of the day after the current date.


*明天的日期*

```go
import "time"
t := time.Now().Add(24 * time.Hour).Format("2006-01-02")
```

---

```rust
fn main() {
    let t = chrono::Utc::now().date().succ().to_string();
    println!("{}", t);
}

```

`2021-07-18UTC`

<br>

### 185. <font color="0c0a3e">Execute function in 30 seconds</font>

> Schedule the execution of f(42) in 30 seconds.


*30秒内执行功能*

```go
import "time"
timer := time.AfterFunc(
	30*time.Second,
	func() {
		f(42)
	})
```


or

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("Scheduling f(42)")

	go func() {
		time.Sleep(3 * time.Second)
		f(42)
	}()

	// Poor man's waiting of completion of f.
	// Don't do this in prod, use proper synchronization instead.
	time.Sleep(4 * time.Second)
}

func f(i int) {
	fmt.Println("Received", i)
}

```

`Scheduling f(42)`




---

```rust
use std::time::Duration;
use std::thread::sleep;
sleep(Duration::new(30, 0));
f(42);
```

<br>

### 186. <font color="0c0a3e">Exit program cleanly</font>

> Exit a program cleanly indicating no error to OS

*干净地退出程序*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	fmt.Println("A")
	os.Exit(0)
	fmt.Println("B")
}

```

`A`

or

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	process1()
	process2()
	process3()
}

func process1() {
	fmt.Println("process 1")
}

func process2() {
	fmt.Println("process 2")
	defer fmt.Println("A")
	defer os.Exit(0)
	defer fmt.Println("B")
	fmt.Println("C")
}

func process3() {
	fmt.Println("process 3")
}
```

```go
process 1
process 2
C
B
```

---

```rust
use std::process::exit;

fn main() {
    println!("A");
    exit(0);
    println!("B");
}
```

`A`




<br>

### 189. <font color="0c0a3e">Filter and transform list</font>


> Produce a new list y containing the result of function T applied to all elements e of list x that match the predicate P.


*过滤和转换列表*

```go
package main

import (
	"fmt"
)

func P(e int) bool {
	// Predicate "is even"
	return e%2 == 0
}

type Result = int

func T(e int) Result {
	// Transformation "square"
	return e * e
}

func main() {
	x := []int{4, 5, 6, 7, 8, 9, 10}

	var y []Result
	for _, e := range x {
		if P(e) {
			y = append(y, T(e))
		}
	}

	fmt.Println(y)
}

```

`[16 36 64 100]`

---

```rust
let y = x.iter()
	.filter(P)
        .map(T)
	.collect::<Vec<_>>();
```

<br>

### 190. <font color="0c0a3e">Call an external C function</font>

> Declare an external C function with the prototype<br>void foo(double *a, int n);<br>and call it, passing an array (or a list) of size 10 to a and 10 to n.<br>Use only standard features of your language.



*调用外部C函数*

```go
// void foo(double *a, int n);
// double a[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
import "C"

C.foo(C.a, 10)
```

---

```rust
extern "C" {
    /// # Safety
    ///
    /// `a` must point to an array of at least size 10
    fn foo(a: *mut libc::c_double, n: libc::c_int);
}

let mut a = [0.0, 1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0];
let n = 10;
unsafe {
    foo(a.as_mut_ptr(), n);
}
```

<br>

### 191. <font color="0c0a3e">Check if any value in a list is larger than a limit</font>


> 	Given a one-dimensional array a, check if any value is larger than x, and execute the procedure f if that is the case


*检查列表中是否有任何值大于限制*

```go
package main

import (
	"fmt"
)

func f() {
	fmt.Println("Larger found")
}

func main() {
	a := []int{1, 2, 3, 4, 5}
	x := 4
	for _, v := range a {
		if v > x {
			f()
			break
		}
	}
}

```

`Larger found`


---

```rust
fn main() {
    let a = [5, 6, 8, -20, 9, 42];

    let x = 35;
    if a.iter().any(|&elem| elem > x) {
        f()
    }

    let x = 50;
    if a.iter().any(|&elem| elem > x) {
        g()
    }
}

fn f() {
    println!("F")
}

fn g() {
    println!("G")
}

```

`F`

<br>

### 192. <font color="0c0a3e">Declare a real variable with at least 20 digits</font>


> Declare a real variable a with at least 20 digits; if the type does not exist, issue an error at compile time.


*声明一个至少有20位数字的实变量*

```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	a, _, err := big.ParseFloat("123456789.123456789123465789", 10, 200, big.ToZero)
	if err != nil {
		panic(err)
	}

	fmt.Println(a)
}

```

`1.234567891234567891234657889999999999999999999999999999999999e+08`

---

```rust
use rust_decimal::Decimal;
use std::str::FromStr;
let a = Decimal::from_str("1234567890.123456789012345").unwrap();
```

<br>




### 197. <font color="0c0a3e"> Get a list of lines from a file</font>


> Retrieve the contents of file at path into a list of strings lines, in which each element is a line of the file.


*从文件中获取行列表.将文件路径中的内容检索到字符串行列表中，其中每个元素都是文件的一行。*

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"strings"
)

func readLines(path string) ([]string, error) {
	b, err := ioutil.ReadFile(path)
	if err != nil {
		return nil, err
	}
	lines := strings.Split(string(b), "\n")
	return lines, nil
}

func main() {
	lines, err := readLines("/tmp/file1")
	if err != nil {
		log.Fatalln(err)
	}

	for i, line := range lines {
		fmt.Printf("line %d: %s\n", i, line)
	}
}

func init() {
	data := []byte(`foo
bar
baz`)
	err := ioutil.WriteFile("/tmp/file1", data, 0644)
	if err != nil {
		log.Fatalln(err)
	}
}

```

```go
line 0: foo
line 1: bar
line 2: baz
```

---

```rust
use std::fs::File;
use std::io::prelude::*;
use std::io::BufReader;

fn main() {
    let path = "/etc/hosts";

    let lines = BufReader::new(File::open(path).unwrap())
        .lines()
        .collect::<Vec<_>>();

    println!("{:?}", lines);
}

```


`[Ok("127.0.0.1\tlocalhost"), Ok("::1\tlocalhost ip6-localhost ip6-loopback"), Ok("fe00::0\tip6-localnet"), Ok("ff00::0\tip6-mcastprefix"), Ok("ff02::1\tip6-allnodes"), Ok("ff02::2\tip6-allrouters")]`

<br>

### 198. <font color="0c0a3e">Abort program execution with error condition</font>


> Abort program execution with error condition x (where x is an integer value)


*出现错误情况时中止程序执行*

```go
package main

import (
	"os"
)

func main() {
	x := 1
	os.Exit(x)
}

```

*Program exited: status 1.*

---

```rust
use std::process;
process::exit(x);
```



<br>

### 200. <font color="0c0a3e">Return hypotenuse</font>

> Returns the hypotenuse h of the triangle where the sides adjacent to the square angle have lengths x and y.




*返回三角形的斜边h，其中与直角相邻的边的长度为x和y。*

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	x := 1.0
	y := 1.0

	h := math.Hypot(x, y)
	fmt.Println(h)
}
```

`1.4142135623730951`

---

```rust
fn main() {
    let (x, y) = (1.0, 1.0);
    let h = hypot(x, y);
    println!("{}", h);
}

fn hypot(x: f64, y: f64) -> f64 {
    let num = x.powi(2) + y.powi(2);
    num.powf(0.5)
}

```

`1.4142135623730951`

<br>

