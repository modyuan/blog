**前言：** 基本上是照搬golang官方的教程，以此来巩固自己的记忆。

## 包
package 用来声明包的名字，程序入口的包名总是main

## 导入
使用import来导入包,两种方式。
```go
import "fmt"
import (
    "math"
    "net"
)
```

## 导出名
导入一个包以后，可以用其导出的名称来调用它。
首字母大写的名称是被导出的。（相当于首字母小写的函数以及变量是private）

## 函数
在golang 中定义一个函数的方法如下
```go
func add(x int, y int) int {
    return x+y
}
```

- 可以看出关键字是func，定义参数的时候形参名在前，类型在后，再往后是返回值类型。  
- golang中，没有像c/c++那样在语句末尾加分号。  
- 同时，`x int, y int`还可以缩写为`x, y int`
- 函数可以返回任意数量的返回值
```
func swap(x, y string)(string,string){
    return y,x
}
```
- 命名返回值，也就是可以提前声明返回值的名字，这样的话语义更加清晰，而且在return时也可以忽略参数，直接返回。
```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```

## 变量
var语句用来定义变量的列表，和定义函数的形参列表一样，类型在后
```go
var c,python,java bool
```
var可以在函数中定义，也可以在包的级别定义

## 变量初始化
变量定义可以同时初始化，每个变量对应一个。  
如果初始化时使用表达式，则可以省略类型，从初始值中获取类型。
```go
var i, j int = 1, 2

var c, python, java = true, false, "no!"
```
在定义一个变量但不指定其类型时（使用没有类型的 var 或 := 语句）， 变量的类型由右值推导得出。

如果定义的时候没有初始化，则数值型为0，布尔型为false，字符串为空字符串。
## 短声明
用`:=`可以代替var定义，这种定义方式不能用在函数外面。


## 基本类型
```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
     // 代表一个Unicode码

float32 float64

complex64 complex128
```
- 双引号`"`用来创建 可解析的字符串字面量 (支持转义，但不能用来引用多行)；
- 反引号 &#96; 用来创建 原生的字符串字面量 ，这些字符串可能由多行组成(不支持任何转义序列)，原生的字符串字面量多用于书写多行消息、HTML以及正则表达式。
- 单引号 `'`一般用来表示「rune literal」 ，即——码点字面量。

## 类型转换
表达式 T(v) 将值 v 转换为类型 `T`。
与C语言不同，不能隐式转换。  
一些关于数值的转换：
```
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```
或者，更加简单的形式：
```
i := 42
f := float64(i)
u := uint(f)
```
## 常量
和变量的定义方法类似，使用const关键字，但是不能用:=语法。
```
const PI=3.1415
```

----


## 循环结构for
go只有一种循环结构for  
基本的例子如下，和C类似，只是没有小括号，同时大括号时必须的。
```go
for i := 0; i < 10; i++ {
		sum += i
}
```
和C或者java一样，可以让前置、后置语句为空
```
for ; sum < 1000; {
	sum += sum
}
```
在上面这种情况下，可以省略分号，这是就退化成了类似while（如下）
```
//这是就变成了c语言中的while，但是节省了关键字
for  sum < 1000 {
	sum += sum
}
```

也可以省略所有控制条件，这是退化成为死循环
```
for {
    //somethins
}
```

## 条件结构if
和for的改动差不多，也是相对于C和java而言，不加小括号，大括号不能省略。
```go
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}
```

这里if可以在条件之前执行一个简单的语句。由这个语句定义的变量作用域仅在if范围之内(在else块中也可以使用)。
```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}
```

## 分支结构switch
和c语言类型，
- 没小括号- 
- 也可以在判断前额外执行一条语句（和上面的if语句一样）
- 分支自动终止，不用加break
- 分支以`fallthrough`语句结束，则会继续往下执行。
```
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
		fmt.Printf("%s.", os)
	}
}
```
没有条件的switch语句，用来代替长的if else链条。
```
func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

## 返回前执行defer
defer 语句会延迟函数的执行直到上层函数返回。

延迟调用的参数会立刻生成，但是在上层函数返回前函数都不会被调用。
多个defer会按照先进后出的顺序执行。
```go
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```
----

## 指针
Go具有指针，指针保存了变量的内存地址。但没有指针运算。  
类型`*T`是指向类型T的指针，其零值是`nil`
有引用运算符`&`，和解引用运算符`*`
```go
var p *int

i:=42
p=&i
*p=43
fmt.Println(*p)
```

## 结构体
结构体就是字段的集合，定义方式如下
```
type Vertex struct {
	X int
	Y int
}

func main() {
    v := Vertex{1, 2}
	fmt.Println(v)
	
	vp := &v
	fmt.Println(vp.X)
}

```
字段访问采用点运算符 `v.X`
结构体指针也可以用点运算符，使用时会自动解引用。

## 结构体文法
结构体文法表示通过结构体字段的值作为列表来新分配一个结构体。

使用 Name: 语法可以仅列出部分字段。（字段名的顺序无关。）

特殊的前缀 & 返回一个指向结构体的指针。
```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 类型为 Vertex
	v2 = Vertex{X: 1}  // Y:0 被省略
	v3 = Vertex{}      // X:0 和 Y:0
	p  = &Vertex{1, 2} // 类型为 *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```


## 数组
类型`[n]T` 是有n个类型为T的值的数组

例如`var a [10]int`  
数组的长度是类型的一部分。

## 切片slice
一个slice会指向一个数组的部分。是一个数组的部分引用。
`[]T`是一个元素类型为T的slice  
slice可以重新切片，创建一个新的slice指向相同的数组。  
`s[a:b]` 相当于一个数组偏移a到b的左闭右开区间

## 构造slice
slice 由函数 make 创建。这会分配一个零长度的数组并且返回一个 slice 指向这个数组：

```
a := make([]int, 5)  // len(a)=5
```
为了指定容量，可传递第三个参数到 `make`：
```
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

slice 的零值是 `nil`。  
一个 nil 的 slice 的长度和容量是 0。

向slice添加元素使用内建函数`append`
```
func append(s []T, vs ...T) []T
```
append 的第一个参数 s 是一个类型为 T 的数组，其余类型为 T 的值将会添加到 slice。

append 的结果是一个包含原 slice 所有元素加上新添加的元素的 slice。

如果 s 的底层数组太小，而不能容纳所有值时，会分配一个更大的数组。 返回的 slice 会指向这个新分配的数组。(注意！此时s指向的数组和在此之前指向的数组发生了变化，若之前有对此的引用，在append后就失效了)

## range
for 循环的 range 格式可以对 slice 或者 map 进行迭代循环。
```
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```
可以通过赋值给 _ 来忽略序号和值。

如果只需要索引值，去掉“, value”的部分即可。
```
func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i)
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```

## map
map映射键到值，相当于一个哈希表。  
map 在使用之前必须用 make 而不是 new 来创建；值为 nil 的 map 是空的，并且不能赋值。

```
type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```
map 的初始化文法跟结构体文法相似，不过必须有键名。
```
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```
如果顶级的类型只有类型名的话，可以在文法的元素中省略键名。
```
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

## 修改map
```go
//insert or modify
m[key]=ele

//get element
elem = m[key]

//delete a key
delete(m,key)

//通过双赋值检测某个键存在,如果 key 在 m 中，`ok` 为 true 。否则， ok 为 `false`，并且 elem 是 map 的元素类型的零值。
elem, ok = m[key]
```
## 函数的闭包
函数也是值，也可以赋给变量
```
func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}

	fmt.Println(hypot(3, 4))
}
```
Go 函数可以是闭包的。闭包是一个函数值，它来自函数体的外部的变量引用。 函数可以对这个引用值进行访问和赋值；换句话说这个函数被“绑定”在这个变量上。
```go
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```
-----

## 方法
方法类比cpp或者java中的类方法。
Go没有类，但是可以在类型或者结构体上定义方法(不能对来自其他包的类型或者基础类型定义方法)
方法接收者 出现在 func 关键字和方法名之间的参数中。
```
type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	fmt.Println(v.Abs())
}
```
方法接收者可以是T或者*T，或者有一些好处：
1. 可以避免在每个方法调用中拷贝值
2. 方法可以修改接收者指向的值


## 接口（interface）
接口类型是由一组方法定义的集合。

接口类型的值可以存放实现这些方法的任何值。
```

type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat 实现了 Abser
	a = &v // a *Vertex 实现了 Abser

	// 下面一行，v 是一个 Vertex（而不是 *Vertex）
	// 所以没有实现 Abser。
	a = v

	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
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

类型通过实现那些方法来实现接口。 没有显式声明的必要；所以也就没有关键字“implements“。

隐式接口解藕了实现接口的包和定义接口的包：互不依赖。


一个普遍存在的接口是 fmt 包中定义的 Stringer。

type Stringer interface {
    String() string
}
Stringer 是一个可以用字符串描述自己的类型。`fmt`包 （还有许多其他包）使用这个来进行输出。


## 错误

Go 程序使用 error 值来表示错误状态。

与 fmt.Stringer 类似，`error` 类型是一个内建接口：
```go
type error interface {
    Error() string
}
```
（与 fmt.Stringer 类似，`fmt` 包在输出时也会试图匹配 `error`。）

通常函数会返回一个 error 值，调用的它的代码应当判断这个错误是否等于 `nil`， 来进行错误处理。
```go
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
}
fmt.Println("Converted integer:", i)
```
error 为 nil 时表示成功；非 nil 的 error 表示错误。

### Readers
io 包指定了 io.Reader 接口， 它表示从数据流结尾读取。

Go 标准库包含了这个接口的许多实现， 包括文件、网络连接、压缩、加密等等。

io.Reader 接口有一个 Read 方法：

func (T) Read(b []byte) (n int, err error)
Read 用数据填充指定的字节 slice，并且返回填充的字节数和错误信息。 在遇到数据流结尾时，返回 io.EOF 错误。

例子代码创建了一个 strings.Reader。 并且以每次 8 字节的速度读取它的输出。
```
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
```

### Web 服务器
包 http 通过任何实现了 http.Handler 的值来响应 HTTP 请求：
```
package http

type Handler interface {
    ServeHTTP(w ResponseWriter, r *Request)
}
```
在这个例子中，类型 Hello 实现了 `http.Handler`。
### 图片
Package image 定义了 Image 接口：
```
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
```
**注意**：`Bounds` 方法的 Rectangle 返回值实际上是一个 image.Rectangle， 其定义在 image 包中。

------

## go的协程goroutine

goroutine 是由 Go 运行时环境管理的轻量级线程。

`go f(x, y, z)`  
开启一个新的 goroutine 执行

`f(x, y, z)`  
f ， x ， y 和 z 是当前 goroutine 中定义的，但是在新的 goroutine 中运行 `f`。

goroutine 在相同的地址空间中运行，因此访问共享内存必须进行同步。sync 提供了这种可能，不过在 Go 中并不经常用到，因为有其他的办法。（在接下来的内容中会涉及到。）

## channel 信道

channel 是有类型的管道，可以用 channel 操作符 `<-` 对其发送或者接收值。
```go
ch <- v    // 将 v 送入 channel ch。
v := <-ch  // 从 ch 接收，并且赋值给 v。
```
（“箭头”就是数据流的方向。）

和 map 与 slice 一样，channel 使用前必须创建：
```go
ch := make(chan int)
```
默认情况下，在另一端准备好之前，发送和接收都会阻塞。这使得 goroutine 可以在没有明确的锁或竞态变量的情况下进行同步。

channel 可以是 _带缓冲的_。为 make 提供第二个参数作为缓冲长度来初始化一个缓冲 channel：

```go
ch := make(chan int, 100)
```
向缓冲 channel 发送数据的时候，只有在缓冲区满的时候才会阻塞。当缓冲区清空的时候接受阻塞

## range和close

发送者可以 close 一个 channel 来表示再没有值会被发送了。接收者可以通过赋值语句的第二参数来测试 channel 是否被关闭：当没有值可以接收并且 channel 已经被关闭，那么经过

`v, ok := <-ch`
之后 ok 会被设置为 `false`。

循环 `for i := range c` 会不断从 channel 接收值，直到它被关闭。

注意： 只有发送者才能关闭 channel，而不是接收者。向一个已经关闭的 channel 发送数据会引起 panic。 还要注意： channel 与文件不同；通常情况下无需关闭它们。只有在需要告诉接收者没有更多的数据的时候才有必要进行关闭，例如中断一个 `range`。

## select
select 语句使得一个 goroutine 在多个通讯操作上等待。

select 会阻塞，直到条件分支中的某个可以继续执行，这时就会执行那个条件分支。当多个都准备好的时候，会随机选择一个。

```
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
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
当 select 中的其他条件分支都没有准备好的时候，`default` 分支会被执行。

为了非阻塞的发送或者接收，可使用 default 分支：


