---
title: 'Rust vs Go:常用语法对比(4)'
date: 2021-09-05 21:40:04
tags: Rust
---


<br>

### 61. <font color="ff0000">Get current date</font>

*获取当前时间*

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	d := time.Now()
	fmt.Println("Now is", d)
	// The Playground has a special sandbox, so you may get a Time value fixed in the past.
}

```

`Now is 2009-11-10 23:00:00 +0000 UTC m=+0.000000001`

---

```rust
extern crate time;
let d = time::now();
```

or

```rust
use std::time::SystemTime;

fn main() {
    let d = SystemTime::now();
    println!("{:?}", d);
}
```

`SystemTime { tv_sec: 1526318418, tv_nsec: 699329521 }`

<br>

### 62. <font color="ff8700">Find substring position</font>

*字符串查找*

查找子字符串位置

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	x := "été chaud"

	{
		y := "chaud"
		i := strings.Index(x, y)
		fmt.Println(i)
	}

	{
		y := "froid"
		i := strings.Index(x, y)
		fmt.Println(i)
	}
}
```

*i is the byte index of y in x, not the character (rune) index.
i will be -1 if y is not found in x.*

```go
6
-1
```


---



```rust
fn main() {
    let x = "été chaud";
    
    let y = "chaud";
    let i = x.find(y);
    println!("{:?}", i);
    
    let y = "froid";
    let i = x.find(y);
    println!("{:?}", i);
}
```

```rust
Some(6)
None
```

<br>

### 63. <font color="ffd300">Replace fragment of a string</font>

*替换字符串片段*

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	x := "oink oink oink"
	y := "oink"
	z := "moo"
	x2 := strings.Replace(x, y, z, -1)
	fmt.Println(x2)
}
```

`moo moo moo`

---

```rust
fn main() {
    let x = "lorem ipsum dolor lorem ipsum";
    let y = "lorem";
    let z = "LOREM";

    let x2 = x.replace(&y, &z);
    
    println!("{}", x2);
}
```

`LOREM ipsum dolor LOREM ipsum`


<br>

### 64. <font color="deff0a">Big integer : value 3 power 247</font>

*超大整数*

```go
package main

import "fmt"
import "math/big"

func main() {
	x := new(big.Int)
	x.Exp(big.NewInt(3), big.NewInt(247), nil)
	fmt.Println(x)
}

```

`7062361041362837614435796717454722507454089864783271756927542774477268334591598635421519542453366332460075473278915787`


---

```rust
extern crate num;
use num::bigint::ToBigInt;

fn main() {
    let a = 3.to_bigint().unwrap();
    let x = num::pow(a, 247);
    println!("{}", x)
}
```

`7062361041362837614435796717454722507454089864783271756927542774477268334591598635421519542453366332460075473278915787`



<br>

### 65. <font color="a1ff0a">Format decimal number</font>

*格式化十进制数*

```go
package main

import "fmt"

func main() {
	x := 0.15625
	s := fmt.Sprintf("%.1f%%", 100.0*x)
	fmt.Println(s)
}

```

`15.6%`

---


```rust
fn main() {
    let x = 0.15625f64;
    let s = format!("{:.1}%", 100.0 * x);
    
    println!("{}", s);
}

```

`15.6%`

<br>

### 66. <font color="0aff99">Big integer exponentiation</font>

*大整数幂运算*

```go
package main

import "fmt"
import "math/big"

func exp(x *big.Int, n int) *big.Int {
	nb := big.NewInt(int64(n))
	var z big.Int
	z.Exp(x, nb, nil)
	return &z
}

func main() {
	x := big.NewInt(3)
	n := 5
	z := exp(x, n)
	fmt.Println(z)
}

```

`243`

---




```rust
extern crate num;

use num::bigint::BigInt;

fn main() {
    let x = BigInt::parse_bytes(b"600000000000", 10).unwrap();
    let n = 42%
```

<br>

### 67. <font color="0aefff">Binomial coefficient "n choose k"</font>

> Calculate binom(n, k) = n! / (k! * (n-k)!). Use an integer type able to handle huge numbers.


*二项式系数“n选择k”*

```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	z := new(big.Int)
	
	z.Binomial(4, 2)
	fmt.Println(z)
	
	z.Binomial(133, 71)
	fmt.Println(z)
}

```

```go
6
555687036928510235891585199545206017600
```

---

```rust
extern crate num;

use num::bigint::BigInt;
use num::bigint::ToBigInt;
use num::traits::One;

fn binom(n: u64, k: u64) -> BigInt {
    let mut res = BigInt::one();
    for i in 0..k {
        res = (res * (n - i).to_bigint().unwrap()) /
              (i + 1).to_bigint().unwrap();
    }
    res
}

fn main() {
    let n = 133;
    let k = 71;

    println!("{}", binom(n, k));
}

```

`555687036928510235891585199545206017600`



<br>

### 68. <font color="147df5">Create a bitset</font>

*创建位集合*

```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	var x *big.Int = new(big.Int)

	x.SetBit(x, 42, 1)

	for _, y := range []int{13, 42} {
		fmt.Println("x has bit", y, "set to", x.Bit(y))
	}
}
```

```go
x has bit 13 set to 0
x has bit 42 set to 1
```

or

```go
package main

import (
	"fmt"
)

const n = 1024

func main() {
	x := make([]bool, n)

	x[42] = true

	for _, y := range []int{13, 42} {
		fmt.Println("x has bit", y, "set to", x[y])
	}
}
```

```go
x has bit 13 set to false
x has bit 42 set to true
```

or


```go
package main

import (
	"fmt"
)

func main() {
	const n = 1024

	x := NewBitset(n)

	x.SetBit(13)
	x.SetBit(42)
	x.ClearBit(13)

	for _, y := range []int{13, 42} {
		fmt.Println("x has bit", y, "set to", x.GetBit(y))
	}
}

type Bitset []uint64

func NewBitset(n int) Bitset {
	return make(Bitset, (n+63)/64)
}

func (b Bitset) GetBit(index int) bool {
	pos := index / 64
	j := index % 64
	return (b[pos] & (uint64(1) << j)) != 0
}

func (b Bitset) SetBit(index int) {
	pos := index / 64
	j := index % 64
	b[pos] |= (uint64(1) << j)
}

func (b Bitset) ClearBit(index int) {
	pos := index / 64
	j := index % 64
	b[pos] ^= (uint64(1) << j)
}

```

```go
x has bit 13 set to false
x has bit 42 set to true
```

---



```rust
fn main() {
    let n = 20;

    let mut x = vec![false; n];

    x[3] = true;
    println!("{:?}", x);
}

```

`[false, false, false, true, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false]`

<br>

### 69. <font color="580aff">Seed random generator</font>


> Use seed s to initialize a random generator.

> If s is constant, the generator output will be the same each time the program runs. If s is based on the current value of the system clock, the generator output will be different each time.


*随机种子生成器*


```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	var s int64 = 42
	rand.Seed(s)
	fmt.Println(rand.Int())
}

```

`3440579354231278675`


or


```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	var s int64 = 42
	r := rand.New(rand.NewSource(s))
	fmt.Println(r.Int())
}
```

`3440579354231278675`

---



```rust
use rand::{Rng, SeedableRng, rngs::StdRng};

fn main() {
    let s = 32;
    let mut rng = StdRng::seed_from_u64(s);
    
    println!("{:?}", rng.gen::<f32>());
}
```

`0.35038823`



<br>

### 70. <font color="be0aff">Use clock as random generator seed</font>

> Get the current datetime and provide it as a seed to a random generator. The generator sequence will be different at each run.


*使用时钟作为随机生成器的种子*

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	rand.Seed(time.Now().UnixNano())
	// Well, the playground date is actually fixed in the past, and the
	// output is cached.
	// But if you run this on your workstation, the output will vary.
	fmt.Println(rand.Intn(999))
}

```

`524`


or

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	// Well, the playground date is actually fixed in the past, and the
	// output is cached.
	// But if you run this on your workstation, the output will vary.
	fmt.Println(r.Intn(999))
}
```

`524`

---


```rust
use rand::{Rng, SeedableRng, rngs::StdRng};
use std::time::SystemTime;

fn main() {
    let d = SystemTime::now()
        .duration_since(SystemTime::UNIX_EPOCH)
        .expect("Duration since UNIX_EPOCH failed");
    let mut rng = StdRng::seed_from_u64(d.as_secs());
    
    println!("{:?}", rng.gen::<f32>());
}
```

`0.7326781`

<br>

### 71. <font color="072ac8">Echo program implementation</font>

> Basic implementation of the Echo program: Print all arguments except the program name, separated by space, followed by newline.<br>The idiom demonstrates how to skip the first argument if necessary, concatenate arguments as strings, append newline and print it to stdout.


*实现 Echo 程序*

```go
package main
import "fmt"
import "os"
import "strings"
func main() {
    fmt.Println(strings.Join(os.Args[1:], " "))
}
```


---


```rust
use std::env;

fn main() {
    println!("{}", env::args().skip(1).collect::<Vec<_>>().join(" "));
}
```

or

```rust
use itertools::Itertools;
println!("{}", std::env::args().skip(1).format(" "));
```


<br>

### 74. <font color="60b6fb">Compute GCD</font>

> 	Compute the greatest common divisor x of big integers a and b. Use an integer type able to handle huge numbers.

*计算大整数a和b的最大公约数x。使用能够处理大数的整数类型。*

```go
package main

import "fmt"
import "math/big"

func main() {
	a, b, x := new(big.Int), new(big.Int), new(big.Int)
	a.SetString("6000000000000", 10)
	b.SetString("9000000000000", 10)
	x.GCD(nil, nil, a, b)
	fmt.Println(x)
}

```

`3000000000000`

---


```rust
extern crate num;

use num::Integer;
use num::bigint::BigInt;

fn main() {
    let a = BigInt::parse_bytes(b"6000000000000", 10).unwrap();
    let b = BigInt::parse_bytes(b"9000000000000", 10).unwrap();
    
    let x = a.gcd(&b);
 
    println!("{}", x);
}
```

`3000000000000`

<br>

### 75. <font color="a2d6f9">Compute LCM</font>

*计算大整数a和b的最小公倍数x。使用能够处理大数的整数类型。*


>	Compute the least common multiple x of big integers a and b. Use an integer type able to handle huge numbers.

```go
package main

import "fmt"
import "math/big"

func main() {
	a, b, gcd, x := new(big.Int), new(big.Int), new(big.Int), new(big.Int)
	a.SetString("6000000000000", 10)
	b.SetString("9000000000000", 10)
	gcd.GCD(nil, nil, a, b)
	x.Div(a, gcd).Mul(x, b)
	fmt.Println(x)
}
```

`18000000000000`


---

```rust
extern crate num;

use num::bigint::BigInt;
use num::Integer;

fn main() {
    let a = BigInt::parse_bytes(b"6000000000000", 10).unwrap();
    let b = BigInt::parse_bytes(b"9000000000000", 10).unwrap();
    let x = a.lcm(&b);
    println!("x = {}", x);
}
```

`x = 18000000000000`

<br>

### 76. <font color="cfe57d">Binary digits from an integer</font>


> Create the string s of integer x written in base 2. <br> E.g. 13 -> "1101"

*将十进制整数转换为二进制数字*

```go
package main

import "fmt"
import "strconv"

func main() {
	x := int64(13)
	s := strconv.FormatInt(x, 2)

	fmt.Println(s)
}

```

`1101`

or

```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	x := big.NewInt(13)
	s := fmt.Sprintf("%b", x)

	fmt.Println(s)
}
```

`1101`

---


```rust
fn main() {
    let x = 13;
    let s = format!("{:b}", x);
    
    println!("{}", s);
}
```

`1101`

<br>

### 77. <font color="fcf300">SComplex number</font>


> Declare a complex x and initialize it with value (3i - 2). Then multiply it by i.


*复数*

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	x := 3i - 2
	x *= 1i

	fmt.Println(x)
	fmt.Print(reflect.TypeOf(x))
}

```

```go
(-3-2i)
complex128
```

---


```rust
extern crate num;
use num::Complex;

fn main() {
    let mut x = Complex::new(-2, 3);
    x *= Complex::i();
    println!("{}", x);
}
```


`-3-2i`


<br>

### 78. <font color="ffc600">"do while" loop</font>

> 	Execute a block once, then execute it again as long as boolean condition c is true.


*循环执行*

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	for {
		x := rollDice()
		fmt.Println("Got", x)
		if x == 3 {
			break
		}

	}
}

func rollDice() int {
	return 1 + rand.Intn(6)
}
```

**Go has no do while loop, use the for loop, instead.**

```go
Got 6
Got 4
Got 6
Got 6
Got 2
Got 1
Got 2
Got 3
```

or

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	for done := false; !done; {
		x := rollDice()
		fmt.Println("Got", x)
		done = x == 3
	}
}

func rollDice() int {
	return 1 + rand.Intn(6)
}
```

```go
Got 6
Got 4
Got 6
Got 6
Got 2
Got 1
Got 2
Got 3
```

---



```rust
loop {
    doStuff();
    if !c { break; }
}
```

**Rust has no do-while loop with syntax sugar. Use loop and break.**

<br>



### 79. <font color="fedd00">Convert integer to floating point number</font>


> 	Declare floating point number y and initialize it with the value of integer x .

*整型转浮点型*

*声明浮点数y并用整数x的值初始化它。*

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	x := 5
	y := float64(x)

	fmt.Println(y)
	fmt.Printf("%.2f\n", y)
	fmt.Println(reflect.TypeOf(y))
}
```

```go
5
5.00
float64
```

---

```rust
fn main() {
    let i = 5;
    let f = i as f64;
    
    println!("int {:?}, float {:?}", i, f);
}
```

```rust
int 5, float 5.0
```

<br>



### 80. <font color="ffcb17"> Truncate floating point number to integer</font>


>Declare integer y and initialize it with the value of floating point number x . Ignore non-integer digits of x .
Make sure to truncate towards zero: a negative x must yield the closest greater integer (not lesser).

*浮点型转整型*

```go
package main

import "fmt"

func main() {
	a := -6.4
	b := 6.4
	c := 6.6
	fmt.Println(int(a))
	fmt.Println(int(b))
	fmt.Println(int(c))
}
```

```go
-6
6
6
```


---


```rust
fn main() {
    let x = 41.59999999f64;
    let y = x as i32;
    println!("{}", y);
}
```

`41`

<br>


