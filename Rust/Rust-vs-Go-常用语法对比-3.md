---
title: 'Rust vs Go:常用语法对比(3)'
date: 2021-09-04 20:51:13
tags: Rust
---


<br>

### 41. <font color="54478c">Reverse a string</font>

*反转字符串*

```go
package main

import "fmt"

func Reverse(s string) string {
	runes := []rune(s)
	for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
		runes[i], runes[j] = runes[j], runes[i]
	}
	return string(runes)
}

func main() {
	input := "The quick brown 狐 jumped over the lazy 犬"
	fmt.Println(Reverse(input))
	// Original string unaltered
	fmt.Println(input)
}

```

输出

```go
犬 yzal eht revo depmuj 狐 nworb kciuq ehT
The quick brown 狐 jumped over the lazy 犬
```


---

```rust
let t = s.chars().rev().collect::<String>();
```

or

```rust
fn main() {
    let s = "lorém ipsüm dolör sit amor ❤ ";
    let t: String = s.chars().rev().collect();
    println!("{}", t);
}
```

输出

` ❤ roma tis rölod müspi mérol`

<br>

### 42. <font color="2c699a">Continue outer loop</font>

> Print each item v of list a which in not contained in list b.
For this, write an outer loop to iterate on a and an inner loop to iterate on b.


*打印列表a中不包含在列表b中的每个项目v。
为此，编写一个外部循环来迭代a，编写一个内部循环来迭代b。*

```go
package main

import "fmt"

func printSubtraction(a []int, b []int) {
mainloop:
	for _, v := range a {
		for _, w := range b {
			if v == w {
				continue mainloop
			}
		}
		fmt.Println(v)
	}
}

func main() {
	a := []int{1, 2, 3, 4}
	b := []int{2, 4, 6, 8}
	printSubtraction(a, b)
}

```
**mainloop is a label used to refer to the outer loop.**

输出

```go
1
3
```


---



```rust
fn main() {
    let a = ['a', 'b', 'c', 'd', 'e'];
    let b = [     'b',      'd'     ];
    
    'outer: for va in &a {
        for vb in &b {
            if va == vb {
                continue 'outer;
            }
        }
        println!("{}", va);
    }
}

```

**'outer is a label used to refer to the outer loop. Labels in Rust start with a '.**

输出

```rust
a
c
e
```

<br>

### 43. <font color="048ba8">Break outer loop</font>

> Look for a negative value v in 2D integer matrix m. Print it and stop searching.

*在2D整数矩阵m中寻找一个负值v，打印出来，停止搜索。*

```go
package main

import "fmt"
import "os"

var m = [][]int{
	{1, 2, 3},
	{11, 0, 30},
	{5, -20, 55},
	{0, 0, -60},
}

func main() {
mainloop:
	for i, line := range m {
		fmt.Fprintln(os.Stderr, "Searching in line", i)
		for _, v := range line {
			if v < 0 {
				fmt.Println("Found ", v)
				break mainloop
			}
		}
	}

	fmt.Println("Done.")
}
```

**mainloop is a label used to refer to the outer loop.**

输出

```go
Searching in line 0
Searching in line 1
Searching in line 2
Found  -20
Done.
```

---


```rust
fn main() {
    let m = vec![
        vec![1, 2, 3],
        vec![11, 0, 30],
        vec![5, -20, 55],
        vec![0, 0, -60],
    ];
    
    'outer: for v in m {
        'inner: for i in v {
            if i < 0 {
                println!("Found {}", i);
                break 'outer;
            }
        }
    }
}

```

**Loop label syntax is similar to lifetimes.**

输出

`Found -20`

<br>

### 44. <font color="0db39e">Insert element in list</font>

> Insert element x at position i in list s. Further elements must be shifted to the right.

*在列表s的位置I插入元素x。其他元素必须向右移动。*

```go
package main

import "fmt"

func main() {

	s := make([]int, 2)
	
	s[0] = 0
	s[1] = 2
	
	fmt.Println(s)	
	// insert one at index one
	s = append(s, 0)
	copy(s[2:], s[1:])
	s[1] = 1
	
	fmt.Println(s)
}
```

输出

```go
[0 2]
[0 1 2]
```

---

```rust
fn main() {
    let mut vec = vec![1, 2, 3];
    vec.insert(1, 4);
    assert_eq!(vec, [1, 4, 2, 3]);
    vec.insert(4, 5);
    assert_eq!(vec, [1, 4, 2, 3, 5]);
    
}
```

<br>

### 45. <font color="16db93">Pause execution for 5 seconds</font>

*在继续下一个指令之前，在当前线程中休眠5秒钟。*

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	time.Sleep(5 * time.Second)
	fmt.Println("Done.")
}

```

---

```rust
use std::{thread, time};
thread::sleep(time::Duration::from_secs(5));
```

<br>

### 46. <font color="83e377">Extract beginning of string (prefix)</font>

> Create string t consisting of the 5 first characters of string s.
Make sure that multibyte characters are properly handled.

*创建由字符串s的前5个字符组成的字符串t。
确保正确处理多字节字符。*

```go
package main

import "fmt"

func main() {
	s := "Привет"
	t := string([]rune(s)[:5])
	
	fmt.Println(t)
}

```

输出

```go
Приве
```

---


```rust
fn main() {
    let s = "été 😁 torride";
    
    let t = s.char_indices().nth(5).map_or(s, |(i, _)| &s[..i]);

    println!("{}", t);
}
```

输出

`été 😁`

<br>

### 47. <font color="b9e769">Extract string suffix</font>

> Create string t consisting in the 5 last characters of string s.
Make sure that multibyte characters are properly handled.

*创建由字符串s的最后5个字符组成的字符串t。
确保正确处理多字节字符*

```go
package main

import "fmt"

func main() {
	s := "hello, world! 문자"
	t := string([]rune(s)[len([]rune(s))-5:])
	fmt.Println(t)
}

```

输出

`d! 문자`

---

```rust
fn main() {
    let s = "tükörfúrógép";
    let last5ch = s.chars().count() - 5;
    let s2: String = s.chars().skip(last5ch).collect();
    println!("{}", s2);
}
```

输出 

`rógép`

<br>

### 48. <font color="efea5a">Multi-line string literal</font>

> Assign to variable s a string literal consisting in several lines of text, including newlines.

*给变量s赋值一个由几行文本组成的字符串，包括换行符。*

```go
package main

import (
	"fmt"
)

func main() {
	s := `Huey
Dewey
Louie`

	fmt.Println(s)
}

```

输出

```go
Huey
Dewey
Louie
```


---


```rust
fn main() {
    let s = "line 1
line 2
line 3";
    
    print!("{}", &s);
}
```

输出

```rust
line 1
line 2
line 3
```

or

```rust
fn main() {
    let s = r#"Huey
Dewey
Louie"#;
    
    print!("{}", &s);
}
```

输出

```rust
Huey
Dewey
Louie
```

<br>

### 49. <font color="f1c453">Split a space-separated string</font>


*拆分用空格分隔的字符串*


> Build list chunks consisting in substrings of input string s, separated by one or more space characters.


*构建由输入字符串的子字符串组成的列表块，由一个或多个空格字符分隔。*




```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "Un dos tres"
	chunks := strings.Split(s, " ")
	fmt.Println(len(chunks))
	fmt.Println(chunks)

	s = " Un dos tres "
	chunks = strings.Split(s, " ")
	fmt.Println(len(chunks))
	fmt.Println(chunks)

	s = "Un  dos"
	chunks = strings.Split(s, " ")
	fmt.Println(len(chunks))
	fmt.Println(chunks)
}

```

输出

```go
3
[Un dos tres]
5
[ Un dos tres ]
3
[Un  dos]
```

or

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "hello world"
	chunks := strings.Fields(s)

	fmt.Println(chunks)
}

```

输出为

```go
[hello world]
```


and


```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "Un dos tres"
	chunks := strings.Fields(s)
	fmt.Println(len(chunks))
	fmt.Println(chunks)

	s = " Un dos tres "
	chunks = strings.Fields(s)
	fmt.Println(len(chunks))
	fmt.Println(chunks)

	s = "Un  dos"
	chunks = strings.Fields(s)
	fmt.Println(len(chunks))
	fmt.Println(chunks)
}
```

输出

```go
3
[Un dos tres]
3
[Un dos tres]
2
[Un dos]
```


strings.Fields 就只能干这个事儿


---



```rust
fn main() {
    let s = "What a  mess";

    let chunks: Vec<_> = s.split_whitespace().collect();

    println!("{:?}", chunks);
}
```

输出

```rust
["What", "a", "mess"]
```


or

```rust
fn main() {
    let s = "What a  mess";

    let chunks: Vec<_> = s.split_ascii_whitespace().collect();

    println!("{:?}", chunks);
}

```
输出

```rust
["What", "a", "mess"]
```


or


```rust
fn main() {
    let s = "What a  mess";

    let chunks: Vec<_> = s.split(' ').collect();

    println!("{:?}", chunks);
}

```

输出

```rust
["What", "a", "", "mess"]
```




<br>

### 50. <font color="f29e4c">Make an infinite loop</font>

*写一个无限循环*

```go
for {
	// Do something
}
```

```go
package main

import "fmt"

func main() {
	k := 0
	for {
		fmt.Println("Hello, playground")
		k++
		if k == 5 {
			break
		}
	}
}

```

输出

```go
Hello, playground
Hello, playground
Hello, playground
Hello, playground
Hello, playground
```

---


```rust
loop {
	// Do something
}
```

<br>

### 51. <font color="e2e2df">Check if map contains key</font>


> Determine whether map m contains an entry for key k

*检查map是否有某个key*

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

	k := "cinco"
	_, ok := m[k]
	fmt.Printf("m contains key %q: %v\n", k, ok)

	k = "tres"
	_, ok = m[k]
	fmt.Printf("m contains key %q: %v\n", k, ok)
}

```

输出

```go
m contains key "cinco": false
m contains key "tres": true
```


---


```rust
use std::collections::HashMap;

fn main() {
    let mut m = HashMap::new();
    m.insert(1, "a");
    m.insert(2, "b");

    let k = 2;

    let hit = m.contains_key(&k);

    println!("{:?}", hit);
}
```

<br>

### 52. <font color="d2d2cf">Check if map contains value</font>

*检查map中是否有某个值*

```go
package main

import (
	"fmt"
)

func containsValue(m map[K]T, v T) bool {
	for _, x := range m {
		if x == v {
			return true
		}
	}
	return false
}

// Arbitrary types for K, T.
type K string
type T int

func main() {
	m := map[K]T{
		"uno":  1,
		"dos":  2,
		"tres": 3,
	}

	var v T = 5
	ok := containsValue(m, v)
	fmt.Printf("m contains value %d: %v\n", v, ok)

	v = 3
	ok = containsValue(m, v)
	fmt.Printf("m contains value %d: %v\n", v, ok)
}
```
 输出

 ```go
m contains value 5: false
m contains value 3: true
```

---

```rust
use std::collections::BTreeMap;

fn main() {
    let mut m = BTreeMap::new();
    m.insert(11, "one");
    m.insert(22, "twenty-two");

    {
        let v = "eight";
        let does_contain = m.values().any(|&val| *val == *v);
        println!("{:?}", does_contain);
    }

    {
        let v = "twenty-two";
        let does_contain = m.values().any(|&val| *val == *v);
        println!("{:?}", does_contain);
    }
}

```

<br>

### 53. <font color="e2cfc4">Join a list of strings</font>

*字符串连接*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {

	x := []string{"xxx", "bbb", "aaa"}
	y := strings.Join(x, "&")

	fmt.Println(y)

}

```

输出

`xxx&bbb&aaa`


关于 [strings.Joins](https://pkg.go.dev/strings#Join)

---



```rust
fn main() {
    let x = vec!["Lorem", "ipsum", "dolor", "sit", "amet"];
    let y = x.join(", ");
    println!("{}", y);
}
```

输出

```rust
Lorem, ipsum, dolor, sit, amet
```
<br>

### 54. <font color="f7d9c4">Compute sum of integers</font>

*计算整数之和*

```go
package main

import "fmt"

func main() {
	x := []int{1, 2, 3}
	s := 0
	for _, v := range x {
		s += v
	}
	fmt.Println(s)
}
```
输出

`6`

---


```rust
fn main() {
    let x: Vec<usize> = (0..=10_000).collect();
    
    eprintln!("Sum of 0-10,000 = {}", x.iter().sum::<usize>())
}
```

输出

`Sum of 0-10,000 = 50005000`



<br>

### 55. <font color="faedcb">Convert integer to string</font>

*将整数转换为字符串*




```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	var i int = 1234
	s := strconv.Itoa(i)
	fmt.Println(s)
}
```

输出

`1234`

or

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	var i int64 = 1234
	s := strconv.FormatInt(i, 10)
	fmt.Println(s)
}
```


输出

`1234`


or

```go
package main

import "fmt"
import "math/big"

func main() {
	var i int = 1234
	s := fmt.Sprintf("%d", i)
	fmt.Println(s)

	var j int = 5678
	s = fmt.Sprintf("%d", j)
	fmt.Println(s)

	var k *big.Int = big.NewInt(90123456)
	s = fmt.Sprintf("%d", k)
	fmt.Println(s)
}

```

输出

```go
1234
5678
90123456
```


---


```rust
fn main() {
    let i = 123;
    let s = i.to_string();

    println!("{}", s);
}
```
输出 

`123`

or

```rust
fn main() {
    let i = 123;
    let s = format!("{}", i);

    println!("{}", s);
}

```

输出 

`123`



<br>

### 56. <font color="c9e4de">Launch 1000 parallel tasks and wait for completion</font>

> Fork-join : launch the concurrent execution of procedure f with parameter i from 1 to 1000.
Tasks are independent and f(i) doesn't return any value.
Tasks need not run all at the same time, so you may use a pool.
Wait for the completion of the 1000 tasks and then print "Finished".


*创建1000个并行任务，并等待其完成*

```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

func f(i int) {
	d := rand.Int() % 10000
	time.Sleep(time.Duration(d))
	fmt.Printf("Hello %v\n", i)
}

func main() {
	var wg sync.WaitGroup
	wg.Add(1000)
	for i := 1; i <= 1000; i++ {
		go func(i int) {
			f(i)
			wg.Done()
		}(i)
	}
	wg.Wait()
	fmt.Println("Finished")
}

```

输出

```go
Hello 741
Hello 651
Hello 49
...（共计1000个）
Hello xxx
```

---


```rust
use std::thread;

fn f(i: i32) {
    i + 1;
}

fn main() {
    let threads: Vec<_> = (0..10).map(|i| thread::spawn(move || f(i))).collect();

    for t in threads {
    	t.join();
    }
}
```



<br>

### 57. <font color="c6def1">Filter list</font>

> Create list y containing items from list x satisfying predicate p. Respect original ordering. Don't modify x in-place.

*过滤list中的值*



```go
package main

import "fmt"

type T int

func main() {
	x := []T{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	p := func(t T) bool { return t%2 == 0 }

	y := make([]T, 0, len(x))
	for _, v := range x {
		if p(v) {
			y = append(y, v)
		}
	}

	fmt.Println(y)
}

```


or

```go
package main

import "fmt"

type T int

func main() {
	x := []T{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	p := func(t T) bool { return t%2 == 0 }

	n := 0
	for _, v := range x {
		if p(v) {
			n++
		}
	}
	y := make([]T, 0, n)
	for _, v := range x {
		if p(v) {
			y = append(y, v)
		}
	}

	fmt.Println(y)
}
```

输出

```go
[2 4 6 8 10]
```

---

```rust
fn main() {
    let x = vec![1, 2, 3, 4, 5, 6];

    let y: Vec<_> = x.iter()
        .filter(|&x| x % 2 == 0)
        .collect();

    println!("{:?}", y);
}
```

输出

```rust
[2, 4, 6]
```

<br>

### 58. <font color="dbcdf0">Extract file content to a string</font>

*提取字符串的文件内容*

```go
package main

import "fmt"
import "io/ioutil"

func main() {
	f := "data.txt"
	b, err := ioutil.ReadFile(f)
	if err != nil {
		panic(err)
	}
	lines := string(b)

	fmt.Println(lines)
}

// Create file in fake FS of the Playground. init is executed before main.
func init() {
	err := ioutil.WriteFile("data.txt", []byte(`Un
Dos
Tres`), 0644)
	if err != nil {
		panic(err)
	}
}

```

输出

```go
Un
Dos
Tres
```

---

```rust
use std::fs::File;
use std::io::prelude::*;

fn main() -> Result<(), ()> {
    let f = "Cargo.toml";

    let mut file = File::open(f).expect("Can't open file.");
    let mut lines = String::new();
    file.read_to_string(&mut lines)
        .expect("Can't read file contents.");

    println!("{}", lines);

    Ok(())
}

```

or 

```rust
use std::fs;

fn main() {
    let f = "Cargo.toml";

    let lines = fs::read_to_string(f).expect("Can't read file.");

    println!("{}", lines);
}
```

<br>

### 59. <font color="f2c6de">Write to standard error stream</font>

> Print the message "x is negative" to standard error (stderr), with integer x value substitution (e.g. "-2 is negative").

*写入标准错误流*

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	x := -2
	fmt.Fprintln(os.Stderr, x, "is negative")
}
```

输出

```go
-2 is negative
```

---


```rust
fn main() {
    let x = -3;
    eprintln!("{} is negative", x);
}

```

输出

```rust
-3 is negative
```



<br>

### 60. <font color="f9c6c9">Read command line argument</font>

*读取命令行参数*

```go
import "os"
x := os.Args[1]
```

---

```rust
use std::env;

fn main() {
    let first_arg = env::args().skip(1).next();

    let fallback = "".to_owned();
    let x = first_arg.unwrap_or(fallback);
    
    println!("{:?}", x);
}
```

输出

`""`

<br>


