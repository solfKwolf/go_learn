# 包

每个 Go 程序都是由包构成的。

程序从 main 包开始运行。

本程序通过导入路径 "fmt" 和 "math/rand" 来使用这两个包。

按照约定，包名与导入路径的最后一个元素一致。例如，"math/rand" 包中的源码均以 package rand 语句开始。

_注意：_ 此程序的运行环境是固定的，因此 rand.Intn 总是会返回相同的数字。 （要得到不同的数字，需为生成器提供不同的种子数，参见 rand.Seed。 练习场中的时间为常量，因此你需要用其它的值作为种子数。）

```
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    fmt.Println("My favorite number is", rand.Intn(10))
}
```

# 导入

此代码用圆括号组合了导入，这是"分组"形式的导入语句。

当然你也可以编写多个导入语句，例如：

```
import "fmt"
import "math"
```

不过使用分组导入语句是更好的形式。

```
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Printf("Now you have %g problems.\n", math.Sqrt(7))
}
```

# 导出名

在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的。例如，Pizza 就是个已导出名，Pi 也同样，它导出自 math 包。

pizza 和 pi 并未以大写字母开头，所以它们是未导出的。

在导入一个包时，你只能引用其中已导出的名字。任何"未导出"的名字在该包外均无法访问。

执行代码，观察错误输出。

然后将 math.pi 改名为 math.Pi 再试着执行一次。

```
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Println(math.Pi)
}
```

# 函数

函数可以没有参数或接受多个参数。

在本例中，add 接受两个 int 类型的参数。

注意类型在变量名 之后。

```
package main

import "fmt"

func add(x int, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}
```

当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略。

```
package main

import "fmt"

func add(x, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}
```

# 多值返回

```
package main

import "fmt"

func swap(xm y string)(string, string) {
  return y,x
}

func main() {
  a, b := swap("hello", "world");
  fmt.Println(a, b)
}
```

# 命名返回值

Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。

返回值的名称应当具有一定的意义，它可以作为文档使用。

没有参数的 return 语句返回已命名的返回值。也就是 直接 返回。

直接返回语句应当仅用在下面这样的短函数中。在长的函数中它们会影响代码的可读性。

```
package main

import "fmt"

func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}

func main() {
    fmt.Println(split(17))
}

// 7 10
```

# 变量

var 语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后。

就像在这个例子中看到的一样，var 语句可以出现在包或函数级别。

```
package main

import "fmt"

var c, python, java bool

func main() {
    var i int
    fmt.Println(i, c, python, java)
}

// 0 false false false
```

# 变量的初始化

变量声明可以包含初始值，每个变量对应一个。

如果初始化值已存在，则可以省略类型；变量会从初始值中获得类型。

```
package main

import "fmt"

var i, j int = 1, 2

func main() {
    var c, python, java = true, false, "no!"
    var a = "aaa"
    fmt.Println(i, j, c, python, java, a)
}

// 1 2 true false no! aaa
```

# 短变量声明

在函数中，简洁赋值语句 := 可在类型明确的地方代替 var 声明。

函数外的每个语句都必须以关键字开始（var, func 等等），因此 := 结构不能在函数外使用。

```
package main

import "fmt"

func main() {
    var i, j int = 1, 2
    k := 3
    c, python, java := true, false, "no!"

    fmt.Println(i, j, k, c, python, java)
}
// 1 2 3 true false no!
```

花里胡哨



# 零值

没有明确初始值的变量声明会被赋予它们的 零值。

零值是：

数值类型为 0， 布尔类型为 false， 字符串为 ""（空字符串）。

```
package main

import "fmt"

func main() {
    var i int
    var f float64
    var b bool
    var s string
    fmt.Printf("%v %v %v %q\n", i, f, b, s)
}

0 0 false ""
```

# 类型转换

表达式 T(v) 将值 v 转换为类型 T。

一些关于数值的转换：

var i int = 42 var f float64 = float64(i) var u uint = uint(f) 或者，更加简单的形式：

i := 42 f := float64(i) u := uint(f) 与 C 不同的是，Go 在不同类型的项之间赋值时需要显式转换。试着移除例子中 float64 或 uint 的转换看看会发生什么。

```
package main

import (
    "fmt"
    "math"
)

func main() {
    var x, y int = 3, 4
    var f float64 = math.Sqrt(float64(x*x + y*y))
    var z uint = uint(f)
    fmt.Println(x, y, z)
}
```

# 类型推导

在声明一个变量而不指定其类型时（即使用不带类型的 := 语法或 var = 表达式语法），变量的类型由右值推导得出。

当右值声明了类型时，新变量的类型与其相同：

var i int j := i // j 也是一个 int 不过当右边包含未指明类型的数值常量时，新变量的类型就可能是 int, float64 或 complex128 了，这取决于常量的精度：

i := 42 // int f := 3.142 // float64 g := 0.867 + 0.5i // complex128 尝试修改示例代码中 v 的初始值，并观察它是如何影响类型的。

# 常量

常量的声明与变量类似，只不过是使用const关键字 常量可以是字符串、字符、布尔值或数值。 常量不能用:=语法声明。

```
package main

import "fmt"

func main() {
  const World = "世界"
  fmt.Println("Hello", World)
  fmt.Println("Happy", Pi, "Day")

  const Truth = true
  fmt.Println("Go rules?", Truth)
}
```

# 数值常量

数值常量是高精度的 值。

一个未指定类型的常量由上下文来决定其类型。

再尝试一下输出 needInt(Big) 吧。

（int 类型最大可以存储一个 64 位的整数，有时会更小。）

（int 可以存放最大64位的整数，根据平台不同有时会更少。）

# for

```
package main
import "fmt"

func main() {
  sum := 0
  for i := 0;i<10; i++ {
    sum += i
  }
  fmt.PrintLn(sum)
}

func main() {
  sum := 1
  for ; sum < 1000; {
    sum += sum
  }
  fmt.Println(sum)
}
```

此时你可以去掉分号，因为 C 的 while 在 Go 中叫做 for。

```
func main() {
  sum := 1
  for sum < 1000  {
    sum += sum
  }
  fmt.Println(sum)
}
```

# 无限循环

```
package main

func main() {
    for {
    }
}
```

# if

```
package main 

import (
  "fmt"
  "math"
)

function sqrt(x float64) string {
  if x < 0 {
    return sqrt(-x) + "i"
  }
  return fmt.Sprint(math.Sqrt(x))
}
```

# if 简短语句

```
func pow(x, n, lim float64) float64 {
    if v := math.Pow(x, n); v < lim {
        return v
    }
    return lim
}

func main() {
    fmt.Println(
        pow(3, 2, 10),
        pow(3, 3, 20),
    )
}
```

# if 和 else

```
if v := math.Pow(x, n); v < lim {
    return v
} else {
    fmt.Printf("%g >= %g\n", v, lim)
}
// 这里开始就不能使用 v 了
return lim
```

## 循环与函数

```
z -= (z*z - x) / (2*z)
```


## switch
switch 是编写一连串 if - else 语句的简便方法。它运行第一个值等于条件表达式的 case 语句。

Go 的 switch 语句类似于 C、C++、Java、JavaScript 和 PHP 中的，不过 Go 只运行选定的 case，而非之后所有的 case。 实际上，Go 自动提供了在这些语言中每个 case 后面所需的 break 语句。 除非以 fallthrough 语句结束，否则分支会自动终止。 Go 的另一点重要的不同在于 switch 的 case 无需为常量，且取值不必为整数。

```
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

## 没有条件switch

```
switch {
  case true: 
    // 执行
  case false:
    // 不执行
  default: 
    // 相当于最后的else匹配
}
```
## defer
defer 语句会将函数推迟到外层函数返回之后执行。

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。


## defer 栈
推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟的函数会按照后进先出的顺序调用。


## 指针

Go 拥有指针。指针保存了值的内存地址。

类型 *T 是指向 T 类型值的指针。其零值为 nil。

```
var p *int
```
& 操作符会生成一个指向其操作数的指针。

```
i := 42
p = &i
```

*操作符表示指针指向的底层值。

```
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```
这也就是通常所说的“间接引用”或“重定向”。

与 C 不同，Go 没有指针运算。


```
func main() {
  i,j := 42, 2701

  p:=&i  // 指向i
  fmt.Println(*p) //通过指针读取i的值
  *p = 21 // 通过指针设置i的值
  fmt.PrintLn(i) // 查看i的值

  p = &j  // 指向j
  *p = *p / 37 // 通过指针对j进行除法计算
  fmt/Println(j) //查看j的值
}
```


## 结构体

一个结构体（struct）就是一组字段（field）。

```
package main

import "fmt"
type Vertex struct {
  X int
  Y int
}

func main() {
  fmt.Println(Vertex{1, 2})
}
{1, 2}

func main() {
  v := Vertex{1, 2}
  v.X = 4
  fmt.Println(v.X)
}

// 4
```

## 结构体指针

结构体字段可以通过结构体指针来访问。

如果我们有一个指向结构体的指针 p，那么可以通过 (*p).X 来访问其字段 X。不过这么写太啰嗦了，所以语言也允许我们使用隐式间接引用，直接写 p.X 就可以。

```
package main

import "fmt"

type Vertex struct {
  X int
  Y int
}

function main() {
  v := Vertex{1, 2}
  p := &v
  p.X = 1e9
  fmt.PrintLn(v)
}
```

## 结构体文法
结构体文法通过直接列出字段的值来新分配一个结构体。

使用 Name: 语法可以仅列出部分字段。（字段名的顺序无关。）

特殊的前缀 & 返回一个指向结构体的指针。

```
package main

import "fmt"

type Vertex struct {
  X, Y int
}

var (
  v1 Vertex {1, 2}
  v2 = Vertex {X: 1}
  v3 = Vertex{}
  p = &Vertex{1, 2}
)

func main() {
  fmt.Println(v1, p, v2, v3);
}

// {1 2} &{1 2} {1 0} {0 0}
```

## 数组

类型 [n]T 表示拥有 n 个 T 类型的值的数组。

```
var a [10]int
```

```
package main

import "fmt"

func main() {
  var a [2]string
  a[0] = "Hello"
  a[1] = "World"
  fmt.Println(a[0], a[1])
  fmt.Println(a)

  primes := [6]int{2,3,5,7,11,13}
  fmt.Println(primes)
}
```

## 切片
每个数组的大小都是固定的。而切片则为数组元素提供动态大小的、灵活的视角。在实践中，切片比数组更常用。


```
package main

import "fmt"

func main() {
  primes := [6]int{2,3,5,7,13}

  var s []int = primes[1:4]
  fmt.Println(s)
}
```

切片就像数组的引用


```
func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
	
	c := []string{"1","222"}
	
	fmt.Println(c)
}
```
## 切片文法

```
package main;

import "fmt"

func main() {
  q := []int{2, 3, 4, 5, 7, 11, 13}
  fmt.Println(q)

  r := []bool {true, false, true, true}
  fmt.Println(r)

  s := []struct {
    i int
    n bool
  } {
    {2, true},
    {3, false}
  }

  fmt.Println(s)
}

```

## 切片的默认行为

```
// 下面切片都是等值的

var a [10]int
a[0:10]
a[:10]
a[0:]
a[:]
```

## 切片的長度与容量

切片s的长度和容量可通过表达式len(s)和cap(s)来获取

长度代表数组实际的长度，容量代表数组可容纳的长度

```
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// 截取切片使其长度为 0
	s = s[:0]
	printSlice(s)

	// 拓展其长度
	s = s[:4]
	printSlice(s)

	// 舍弃前两个值
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fm

// result  
len=6 cap=6 [2 3 5 7 11 13]
len=0 cap=6 []
len=4 cap=6 [2 3 5 7]
len=2 cap=4 [5 7]
```

## nil切片

切片的零值是nil
nil 切片的长度和容量为 0 且没有底层数组。

## 用make创造切片
切片可以用内建函数 make 来创建，这也是你创建动态数组的方式。

make 函数会分配一个元素为零值的数组并返回一个引用了它的切片：

```
a := make([]int, 5)  // len(a)=5
```
要指定它的容量，需要向make传入第3个参数
```
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

## 用make创建切片

```
a := make([]int, 5) // len(a) = 5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

## 切片的切片
有点类似二维数组

```
package main

import (
	"fmt"
	"strings"
)

func main() {
	// 创建一个井字板（经典游戏）
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// 两个玩家轮流打上 X 和 O
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}

X _ X
O _ X
_ _ O
```
## 向切片追加元素
```
func append(s []T, vs ...T) []T

func main() {
	var s []int
	printSlice(s)

	// 添加一个空切片
	s = append(s, 0)
	printSlice(s)

	// 这个切片会按需增长
	s = append(s, 1)
	printSlice(s)

	// 可以一次性添加多个元素
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

len=0 cap=0 []
len=1 cap=2 [0]
len=2 cap=2 [0 1]
len=5 cap=8 [0 1 2 3 4]
```


## Range

```
package main

import "fmt"

var pow = []int {1, 2, 4}

func main() {
  for i, v := range pow {
    fmt.Println("2**%d = %d\n", i, v)
  }
}

```

// i 下标 v value


可以将下标或值赋予 _ 来忽略它。
```
for i, _ := range pow
for _, value := range pow
```
若你只需要索引，忽略第二个变量即可。
```
for i := range pow
```
## 切片 练习

```
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	
	result := make([][]uint8, dy)
	for i:= 0;i<dy;i++ {
		result[i] = make([]uint8, dx)
		for j:=0;j<dx;j++ {
			result[i][j] = uint8((i+j)/2)
		}
	}
	return result;
}

func main() {
	pic.Show(Pic)
}

```

## 映射

映射将键映射到值。

映射的零值为 nil 。nil 映射既没有键，也不能添加键。

make 函数会返回给定类型的映射，并将其初始化备用。


```
package main

import "fmt"

type Vertex struct {
  Lat, Long float64
}

var m map[string] Vertex

func main() {
  m = make(map[string]Vertex)
  m["Bell Labs"] = Vertex {
    40.313,-23.2313,
  }
  fmt.Println(m["Bell Labs"])
}
```


映射的文法
映射的文法与结构体相似，不过必须有键名。

```
package main

import "fmt"

type Vertex struct {
  Lat,Long float63
}


var m = map[string]Vertex{
  "Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```

若顶级类型只是一个类型名，你可以在文法的元素中省略它。

```
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

func main() {
	fmt.Println(m)
}
```

## 修改映射

在映射m中插入或修改元素：
```
m[key] = elem
```
获取元素：
```
elem = m[key]
```
删除元素
```
delete(m, key)
```
通过双赋值检查某个键是否存在
```
elem, ok = m[key]
```

若key在m中，ok在true；否则，ok为false。

```
elem, ok := m[key]
```





## 函数

```
package main

import (
  "fmt"
  "math"
)

func compute(fn func(float64, float64) float64) float64 {
  return fn(3,4)
}


func main() {
  hypot := func(x,y float64) float64 {
    return math.Sqrt(x*x+y*y)
  }


}
```

## 函数的闭包
Go 函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，换句话说，该函数被这些变量“绑定”在一起。

例如，函数 adder 返回一个闭包。每个闭包都被绑定在其各自的 sum 变量上。

```

func adder() func(int) int {
  sum := 0;
  return func(x int) int {
    sum += x
    return sum
  }
}
```

## 方法
```
package main 

import (
  "fmt",
  "math"
)

type Vertex struct {
  X,Y float64
}

func (v Vertex) Abs() float64 {
  return math.Sqrt(v.X*v.X+v.Y*v.Y)
}

func main() {
  v:= Vertex{3, 4}
  fmt.Println(v.Abs())
}
```

## 方法即函数


```
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}

```

你也可以为非结构体类型声明方法。

在此例中，我们看到了一个带 Abs 方法的数值类型 MyFloat。

你只能为在同一包内定义的类型的接收者声明方法，而不能为其它包内定义的类型（包括 int 之类的内建类型）的接收者声明方法。

（译注：就是接收者的类型定义和方法声明必须在同一包内；不能为内建类型声明方法。）

```
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}

```


## 指针的接受者

你可以为指针接收者声明方法。

这意味着对于某类型 T，接收者的类型可以用 *T 的文法。（此外，T 不能是像 *int 这样的指针。）

例如，这里为 *Vertex 定义了 Scale 方法。

指针接收者的方法可以修改接收者指向的值（就像 Scale 在这做的）。由于方法经常需要修改它的接收者，指针接收者比值接收者更常用。

试着移除第 16 行 Scale 函数声明中的 *，观察此程序的行为如何变化。

若使用值接收者，那么 Scale 方法会对原始 Vertex 值的副本进行操作。（对于函数的其它参数也是如此。）Scale 方法必须用指针接受者来更改 main 函数中声明的 Vertex 的值。

```
package main 

import (
  "fmt",
  "math"
)

type Vertex struct {
  X, Y float64
}

func (v Vertex) Abs() float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v Vertex*) Scale(f float64) {
  v.X = v.X * f 
  v.Y = v.Y * f 
}


func main() {
	v := Vertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs())
}

// 50
```

## 方法与指针重定向
比较前两个程序，你大概会注意到带指针参数的函数必须接受一个指针：
```
var v Vertex
ScaleFunc(v, 5)  // 编译错误！
ScaleFunc(&v, 5) // OK
```
而以指针为接收者的方法被调用时，接收者既能为值又能为指针：

```
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```
对于语句 v.Scale(5)，即便 v 是个值而非指针，带指针接收者的方法也能被直接调用。 也就是说，由于 Scale 方法有一个指针接收者，为方便起见，Go 会将语句 v.Scale(5) 解释为 (&v).Scale(5)。

接收一个值为参数的函数必须接受一个指定类型的值:

```
var v Vertex
fmt.Println(AbsFunc(v))
fmt.Println(AbsFunc(&v)) // 编译失败
```
而以值为接收者的方法被调用时，接收者既能为值又能为指针：
```
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```
这种情况下，方法调用 p.Abs() 会被解释为 (*p).Abs()。


## 选择值或指针作为接收者

使用指针接收者的原因有二：

首先，方法能够修改其接收者指向的值。

其次，这样可以避免在每次调用方法时复制该值。若值的类型为大型结构体时，这样做会更加高效。

在本例中，Scale 和 Abs 接收者的类型为 *Vertex，即便 Abs 并不需要修改其接收者。


## 接口

接口类型是由一组方法签名定义的集合。

接口类型的变量可以保存任何实现了这些方法的值。

```
package main 

import Abser interface {
  Abs() float64
}

func main() {
  var a Abser

  f := MyFloat(-math.Sqrt2)
  a = f
  a = &v
}


type MyFloat float64

func (f MyFloat) Abs() float {
  if f<0 {
    return float64(-f)
  }
  return float64(f)
}

type Vertex struct {
  X, Y float64
}

func (v *Vertex) Abs() float64 {
  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

## 接口与隐式实现



```
package main

import "fmt"

type I interface {
  M()
}

type T interface {
  S string
}

// 此方法表示类型 T 实现了接口 I，但我们无需显式声明此事。
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}
```

## 接口值

接口也是值。它们可以像其它值一样传递。

接口值可以用作函数的参数或返回值。

在内部，接口值可以看做包含值和具体类型的元组：

(value, type)
接口值保存了一个具体底层类型的具体值。

接口值调用方法时会执行其底层类型的同名方法。

```
package main

import (
  "fmt"
  "math"
)

type I interface {
  M()
}

type T struct {
  S string
}


func (t *T) M() {
  fmt.Println(t.S)
}

func main() {
  var i I

	i = &T{"Hello"}
	describe(i)
	i.M()

	i = F(math.Pi)
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}

// result
(&{Hello}, *main.T)
Hello
(3.141592653589793, main.F)
3.141592653589793
```


## 底层值为 nil 的接口值

即便接口内的具体值为 nil，方法仍然会被 nil 接收者调用。

在一些语言中，这会触发一个空指针异常，但在 Go 中通常会写一些方法来优雅地处理它（如本例中的 M 方法）。

注意: 保存了 nil 具体值的接口其自身并不为 nil。

```
package main 

import "fmt"

type I interface {
  M()
}

type T struct {
  S string
}

func (t *T) M() {
  if t == nil {
    fmt.Println("<nil>")
    return
  }
  fmt.Println(t.S)
}


```


## nil 接口值
nil 接口值既不保存值也不保存具体类型。

为 nil 接口调用方法会产生运行时错误，因为接口的元组内并未包含能够指明该调用哪个 具体 方法的类型。


## 空接口

```
interface{}


func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}

```


## 类型断言

类型断言 提供访问接口值底层具体值得实现


```
t := i.(T)


t, ok := i.(T)
```


## 类型选择

```
switch v := i.(type) {
case T:
    // v 的类型为 T
case S:
    // v 的类型为 S
default:
    // 没有匹配，v 与 i 的类型相同
}
```

## Stringer
fmt包中定义的Stringer是最普遍的接口之一。

```
type Stringer interface {
    String() string
}
```
Stringer 是一个可以用字符串描述自己的类型。fmt 包（还有很多包）都通过此接口来打印值。

```
package main

import "fmt"

type Person struct {
  Name string
  Age int
}

func (p Person) String() string {
  return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}
```

## 练习

```
package main

import "fmt"

type IPAddr [4]byte

// TODO: 给 IPAddr 添加一个 "String() string" 方法

func (ip  IPAddr) String() string {
	return fmt.Sprintf("%v.%v.%v.%v\n", ip[0],ip[1],ip[2],ip[3])
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}
```

## 错误

Go 程序使用 error 值来表示错误状态。

与 fmt.Stringer 类似，error 类型是一个内建接口：

```
type error interface {
    Error() string
}
```
（与 fmt.Stringer 类似，fmt 包在打印值时也会满足 error。）

通常函数会返回一个 error 值，调用的它的代码应当判断这个错误是否等于 nil 来进行错误处理。

```
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)
```
error 为 nil 时表示成功；非 nil 的 error 表示失败。

```
package main

import (
  "fmt"
  "time"
)

type MyError struct {
  When time.Time
  What string
}

func (e *MyError) Error() string {
  return fmt.Sprintf("at %v, %s", e.When, e.What)
}

func run() error {
  return &MyError{
    time.Now(),
    "it didn't work"
  }
}

func main() {
  if err := run(); err != nil {
    fmt.Println(err)
  }
}
```

## Reader

io包指定了io.Reader接口，它表示从数据流的末尾进行读取。

Go标准库包含了该接口的许多实现，包括文件、网络连接、压缩和加密



```
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}


n = 8 err = <nil> b = [72 101 108 108 111 44 32 82]
b[:n] = "Hello, R"
n = 6 err = <nil> b = [101 97 100 101 114 33 32 82]
b[:n] = "eader!"
n = 0 err = EOF b = [101 97 100 101 114 33 32 82]
b[:n] = ""
```


## Go 程
Go 程（goroutine）是由 Go 运行时管理的轻量级线程。

```
go f(x, y, z)
```
会启动一个新的 Go 程并执行

```
f(x, y, z)
```

f, x, y 和 z 的求值发生在当前的 Go 程中，而 f 的执行发生在新的 Go 程中。

Go 程在相同的地址空间中运行，因此在访问共享的内存时必须进行同步。sync 包提供了这种能力，不过在 Go 中并不经常用到，因为还有其它的办法（见下一页）。

## 信道

信道是带有类型的管道，你可以通过它用信道操作符<-来发送或者接收值

```
ch <- v    // 将 v 发送至信道 ch。
v := <-ch  // 从 ch 接收值并赋予 v。
```

(“箭头”就是数据流的方向.)

```
ch := make(chan int)
```

默认情况下，发送和接收操作在另一端准备好之前都会阻塞。这使得 Go 程可以在没有显式的锁或竞态变量的情况下进行同步。

以下示例对切片中的数进行求和，将任务分配给两个 Go 程。一旦两个 Go 程完成了它们的计算，它就能算出最终的结果。

## 带缓冲的信道


信道可以是 带缓冲的。将缓冲长度作为第二个参数提供给 make 来初始化一个带缓冲的信道：

```
ch := make(chan int, 100)
```
仅当信道的缓冲区填满后，向其发送数据时才会阻塞。当缓冲区为空时，接受方会阻塞。

修改示例填满缓冲区，然后看看会发生什么。


## range 和 close
发送者可通过 close 关闭一个信道来表示没有需要发送的值了。接收者可以通过为接收表达式分配第二个参数来测试信道是否被关闭：若没有值可以接收且信道已被关闭，那么在执行完

```
v, ok := <-ch
```


```
package main

import (
	"fmt"
)

func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i := range c {
		fmt.Println(i)
	}
}
```
## select 语句
select 语句使一个 Go 程可以等待多个通信操作。

select 会阻塞到某个分支可以继续执行为止，这时就会执行该分支。当多个分支都准备好时会随机选择一个执行。

```
package main

import "fmt"

func fibonacci(c, quit chan int) {
  x, y:=0, 1

  for {
    select {
      case c <- x:
        x, y = y, x+y
      case <-quit:
        fmt.Println("quit")
    }
  }
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}


```

## sync.Mutex
我们已经看到信道非常适合在各个 Go 程间进行通信。

但是如果我们并不需要通信呢？比如说，若我们只是想保证每次只有一个 Go 程能够访问一个共享的变量，从而避免冲突？

这里涉及的概念叫做 *互斥（mutual*exclusion）* ，我们通常使用 *互斥锁（Mutex）* 这一数据结构来提供这种机制。

Go 标准库中提供了 sync.Mutex 互斥锁类型及其两个方法：

Lock
Unlock
我们可以通过在代码前调用 Lock 方法，在代码后调用 Unlock 方法来保证一段代码的互斥执行。参见 Inc 方法。

我们也可以用 defer 语句来保证互斥锁一定会被解锁。参见 Value 方法。
```
package main

import (
	"fmt"
	"sync"
	"time"
)

// SafeCounter 的并发使用是安全的。
type SafeCounter struct {
	v   map[string]int
	mux sync.Mutex
}

// Inc 增加给定 key 的计数器的值。
func (c *SafeCounter) Inc(key string) {
	c.mux.Lock()
	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
	c.v[key]++
	c.mux.Unlock()
}

// Value 返回给定 key 的计数器的当前值。
func (c *SafeCounter) Value(key string) int {
	c.mux.Lock()
	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
	defer c.mux.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}

```

## GOPATH设置

go 命令依赖一个重要的环境变量：$GOPATH
GOPATH允许多个目录，当有多个目录时，请注意分隔符，多个目录的时候Windows是分号，Linux系统是冒号，当有多个GOPATH时，默认会将go get的内容放在第一个目录下。

以上 $GOPATH 目录约定有三个子目录：

src 存放源代码（比如：.go .c .h .s等）
pkg 编译后生成的文件（比如：.a）
bin 编译后生成的可执行文件（为了方便，可以把此目录加入到 $PATH 变量中，如果有多个gopath，那么使用${GOPATH//://bin:}/bin添加所有的bin目录）
以后我所有的例子都是以mygo作为我的gopath目录

## 编译应用

```
go build // 编译程序
```
## 获取远程包
go语言有一个获取远程包的工具就是go get，目前go get支持多数开源社区(例如：GitHub、googlecode、bitbucket、Launchpad)
```
go get github.com/astaxie/beedb
```
go get -u 参数可以自动更新包，而且当go get的时候会自动获取该包依赖的其他第三方包

# Go命令

## go build

## go clean

## go fmt

## go get

## go install

## go test
执行这个命令，会自动读取源码目录下面名为*_test.go的文件，生成并运行测试用的可执行文件。输出的信息类似

```
ok   archive/tar   0.011s
FAIL archive/zip   0.022s
ok   compress/gzip 0.033s
```

## go tool

+ go tool fix . 用来修复以前老版本的代码到新版本，例如go1之前老版本的代码转化到go1,例如API的变化
+ go tool vet directory|files 用来分析当前目录的代码是否都是正确的代码,例如是不是调用fmt.Printf里面的参数不正确，例如函数里面提前return了然后出现了无用代码之类的。

这个命令是从Go1.4开始才设计的，用于在编译前自动化生成某类代码。go generate和go build是完全不一样的命令，通过分析源码中特殊的注释，然后执行相应的命令。这些命令都是很明确的，没有任何的依赖在里面。而且大家在用这个之前心里面一定要有一个理念，这个go generate是给你用的，不是给使用你这个包的人用的，是方便你来生成一些代码的。

这里我们来举一个简单的例子，例如我们经常会使用yacc来生成代码，那么我们常用这样的命令：

go tool yacc -o gopher.go -p parser gopher.y
-o 指定了输出的文件名， -p指定了package的名称，这是一个单独的命令，如果我们想让go generate来触发这个命令，那么就可以在当前目录的任意一个xxx.go文件里面的任意位置增加一行如下的注释：

//go:generate go tool yacc -o gopher.go -p parser gopher.y
这里我们注意了，//go:generate是没有任何空格的，这其实就是一个固定的格式，在扫描源码文件的时候就是根据这个来判断的。

所以我们可以通过如下的命令来生成，编译，测试。如果gopher.y文件有修改，那么就重新执行go generate重新生成文件就好。

$ go generate
$ go build
$ go test

## godoc

## 其他命令
```
go version 查看go当前的版本
go env 查看当前go的环境变量
go list 列出当前全部安装的package
go run 编译并运行Go程序
```

## Go开发工具

# Go语言基础

```
break    default      func    interface    select
case     defer        go      map          struct
chan     else         goto    package      switch
const    fallthrough  if      range        type
continue for          import  return       var
```


```
scheme://host[:port#]/path/.../[?query-string][#anchor]
scheme         指定底层使用的协议(例如：http, https, ftp)
host           HTTP服务器的IP地址或者域名
port#          HTTP服务器的默认端口是80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如 http://www.cnblogs.com:8080/
path           访问资源的路径
query-string   发送给http服务器的数据
anchor         锚
```


HTTP协议是建立在TCP协议之上的，因此TCP攻击一样会影响HTTP的通讯，例如比较常见的一些攻击：SYN Flood是当前最流行的DoS（拒绝服务攻击）与DdoS（分布式拒绝服务攻击）的方式之一，这是一种利用TCP协议缺陷，发送大量伪造的TCP连接请求，从而使得被攻击方资源耗尽（CPU满负荷或内存不足）的攻击方式。

## 请求实例

## web工作方式的几个概念

+ Request 用户请求的信息，用来解析用户的请求信息，包括post、get、cookie、url等信息
+ Response 服务器需要反馈给客户端的信息
+ Conn：用户的每次请求链接
+ Handler：处理请求和生成返回信息的处理逻辑

图3.9 http包执行流程

创建Listen Socket, 监听指定的端口, 等待客户端请求到来。

Listen Socket接受客户端的请求, 得到Client Socket, 接下来通过Client Socket与客户端通信。

处理客户端的请求, 首先从Client Socket读取HTTP请求的协议头, 如果是POST方法, 还可能要读取客户端提交的数据, 然后交给相应的handler处理请求, handler处理完毕准备好客户端需要的数据, 通过Client Socket写给客户端。

监控之后如何接收客户端的请求呢？上面代码执行监控端口之后，调用了srv.Serve(net.Listener)函数，这个函数就是处理接收客户端的请求信息。这个函数里面起了一个for{}，首先通过Listener接收请求，其次创建一个Conn，最后单独开了一个goroutine，把这个请求的数据当做参数扔给这个conn去服务：go c.serve()。这个就是高并发体现了，用户的每一次请求都是在一个新的goroutine去服务，相互不影响。


## Go的http包详解
前面小节介绍了Go怎么样实现了Web工作模式的一个流程，这一小节，我们将详细地解剖一下http包，看它到底是怎样实现整个过程的。

Go的http有两个核心功能：Conn、ServeMux

## ServeMux的自定义

```
type ServeMux struct {
  mu sync.RWMutex
  m map[string]muxEntry
  hosts bool
}
```

+ muxEntry

```
type muxEntry struct {
  explicit bool
  h handler 
  pattern string
}
```

+ Handler

```
type Handler interface {
  ServeHTTP(RespnseWriter, *Request)
}
```


r.Form里面包含了所有请求的参数，比如URL中query-string、POST的数据、PUT的数据，所以当你在URL中的query-string字段和POST冲突时，会保存成一个slice，里面存储了多个值，Go官方文档中说在接下来的版本里面将会把POST、GET这些数据分离开来。



## 表单验证

* 必填字段

```
if len(r.Form["username"][0]) == 0 {
  // 为空处理
}

* 数字
getint, err := strconv.Atoi(r.Form.Get("age"))

if err != nil {
  //数字转换出错了，那么可能就不是数字
}

if getint > 100 {
  // 太大了
}
```
还有一种方式就是正则匹配的方式

```

if m, _ = regexp.MatchString("^[0-9]+$", r.Form.Get("age");) !m {
  return false
}

```

* 中文

```
if m, _ := regexp.MatchString("^\\p{Han}+$", r.Form.Get("realname")); !m {
	return false
}
```

* 英文

```
if m, _ := regexp.MatchString("^[a-zA-Z]+$", r.Form.Get("engname")); !m {
  return false
}
```


## 转义

```
fmt.Println("username:", template.HTMLEscapeString(r.Form.Get("username")))
```

输出非转义字符

```
import "text/template"
t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)

err = t.ExecuteTemplate(out, "T", "")
```


## 文件上传

```
通过上面的代码可以看到，处理文件上传我们需要调用r.ParseMultipartForm，
里面的参数表示maxMemory
```

## database/sql接口

### sql.Register

```
//https://github.com/mattn/go-sqlite3驱动
func init() {
	sql.Register("sqlite3", &SQLiteDriver{})
}

//https://github.com/mikespook/mymysql驱动
// Driver automatically registered in database/sql
var d = Driver{proto: "tcp", raddr: "127.0.0.1:3306"}
func init() {
	Register("SET NAMES utf8")
	sql.Register("mymysql", &d)
}
```

我们看到第三方数据库驱动都是通过调用这个函数来注册自己的数据库驱动名称以及相应的driver实现。在database/sql内部通过一个map来存储用户定义的相应驱动。

```
var drivers = make(map[string]driver.Driver)

drivers[name] = driver
```


## 文件

```
var drivers = make(map[string]driver.Driver)

drivers[name] = driver
```

+ driver.Driver


Driver是一个数据库驱动的接口，他定义了给一个method: Open(name string),这个方法返回一个数据库的Conn接口

```
type Driver interfacce {
  Open(name string) (Conn, error)
}
```


## driver.Conn
```
type Conn interface {
	Prepare(query string) (Stmt, error)
	Close() error
	Begin() (Tx, error)
}
```


## driver.Stmt

## driver.Tx

## driver.Execer

## driver.Result

## driver.Rows

## driver.RowsAffected

## driver.Value

## driver.ValueConverter


## Cookie

```

http.SetCookie(w RespnseWriter, cookie *Cookie)
expiration := time.Now()
expiration = expiration.AddDate(1, 0, 0)
cookie := http.Cookie{Name: "username", Value: "astaxie", Expires: expiration}
http.SetCookie(w, &cookie)
```

读取cookie
```
for _, cookie := range r.Cookies() {
  fmt.Fprint(w, cookie.name)
} 
```

## session

7 文本处理
Web开发中对于文本处理是非常重要的一部分，我们往往需要对输出或者输入的内容进行处理，这里的文本包括字符串、数字、Json、XMl等等。Go语言作为一门高性能的语言，对这些文本的处理都有官方的标准库来支持。而且在你使用中你会发现Go标准库的一些设计相当的巧妙，而且对于使用者来说也很方便就能处理这些文本。本章我们将通过四个小节的介绍，让用户对Go语言处理文本有一个很好的认识。

XML是目前很多标准接口的交互语言，很多时候和一些Java编写的webserver进行交互都是基于XML标准进行交互，7.1小节将介绍如何处理XML文本，我们使用XML之后发现它太复杂了，现在很多互联网企业对外的API大多数采用了JSON格式，这种格式描述简单，但是又能很好的表达意思，7.2小节我们将讲述如何来处理这样的JSON格式数据。正则是一个让人又爱又恨的工具，它处理文本的能力非常强大，我们在前面表单验证里面已经有所领略它的强大，7.3小节将详细的更深入的讲解如何利用好Go的正则。Web开发中一个很重要的部分就是MVC分离，在Go语言的Web开发中V有一个专门的包来支持template,7.4小节将详细的讲解如何使用模版来进行输出内容。7.5小节将详细介绍如何进行文件和文件夹的操作。7.6小结介绍了字符串的相关操作。


## XML处理

```
<?xml version="1.0" encoding="utf-8"?>
<servers version="1">
	<server>
		<serverName>Shanghai_VPN</serverName>
		<serverIP>127.0.0.1</serverIP>
	</server>
	<server>
		<serverName>Beijing_VPN</serverName>
		<serverIP>127.0.0.2</serverIP>
	</server>
</servers>
```

## 解析xml

如何解析如上这个XML文件呢？ 我们可以通过xml包的Unmarshal函数来达到我们的目的
```
func Unmarshal(data []byte, v interface{}) error
```



