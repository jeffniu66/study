---
typora-root-url: ./go_images
---

# 1. 第一个go程序

hello.go

```go
// 1.go语言以包作为管理单位
// 2.每个文件必须先声明包
// 3.程序必须有一个main包(重要)
package main

import "fmt" // 导入包后必须要使用，否则编译不通过

func main() { // 这个"{"不能换行
	fmt.Println("Hello World!") // 结尾不需要";"
}
```



# 2. 数据类型

## 2.1 变量声明

````go
// 格式: var 变量名 类型，变量声明了 必须要使用
var a int
// 可以同时声明多个变量
var b, c int
````

## 2.2 变量初始化

````go
// 声明的同时赋值
var a int = 10
// 自动推导类型，必须初始化，通过初始化的值确定类型（常用）
c := 30
fmt.Printf("c type is %T\n", c) // 输出：c type is int
````

## 2.3 自动推导类型和赋值的区别

````go
var a int
a = 10
a = 20
a = 30 // 赋值可以使用n次

b := 20
//b := 30 自动推导只能使用一次
````

## 2.4 多重赋值和匿名变量

````go
// 多重赋值
a, b := 10, 20
// 交换两个变量的值
i, j := 10, 20
i, j = j, i

// _ 匿名变量，丢弃数据不处理，匿名变量配合函数返回值使用才有优势
var c, d, e int 
c, d, e = test() // go可以返回多个值
_, d, _ = test() // 只返回d
````

## 2.5 常量的使用

````go
const b = 20 // 不能使用:= 可以省略类型
````

## 2.6 多个变量或常量的定义

````go
// 多个变量
var (
	a int = 10 // 可以省略类型
  b float64 = 2.0
)
// 多个常量
const (
	i int = 10 // 可以省略类型
  j float64 = 3.14
)
````

## 2.7 iota枚举

````go
// 1.iota常量自动生成器，每隔一行，自动累加1
// 2.iota给常量赋值使用
const (
	a = iota
  b = iota
  c = iota
)
// 3.iota遇到const，重置为0
const d = iota // d为0
// 4.可以只写一个iota
const (
	a1 = iota // 0
  b1
  c1
)
// 5.如果是同一行，值都一样
const (
	i = iota // 值为0
  j1, j2, j3 = iota, iota, iota // 值都为1
  k = iota // 值为2
)
````

## 2.8 基础数据类型

### 2.8.1 字符类型

````go
// go语言声明字符类型用byte
var ch byte
````

### 2.8.2 字符串类型

````go
// 字符串都是隐藏了一个结束符，'\0'
var str string
str = "a" // 由'a'和'\0'组成了一个字符串
str = "hello go"
// 只想操作字符串的某个字符，从0开始操作
fmt.Printf("str[0] = %c, str[1] = %c\n", str[0], str[1]) // 输出h,e
````

## 2.9 变量的输入

````go
var a int
fmt.Printf("请输入变量a: ")

// 阻塞等待用户输入
// fmt.Scanf("%d", &a) // 别忘了&
fmt.Scan(&a)
fmt.Println("a = ", a)
````

## 2.10 类型转换

````go
var ch byte
ch = 'a' // 字符串类型本质上就是整型
var t int
t = int(ch) // 类型转换，把ch的值取出来后，转成int再给t赋值
fmt.Println("t = ", t)
````

## 2.11 类型别名

````go
// 给int64起一个别名叫bigint
type bigint int64

var a bigint // 等价于var a int64

type(
	long int64
  char byte
)
var a long = 11
var ch char = 'a'
fmt.Prinf("b = %d, ch = %c\n", b, ch)
````

# 3. 流程控制

## 3.1 if使用

````go
s := "王思聪"
if s == "王思聪" { // if后面不需要括号
}

// if支持1个初始化语句，初始化语句和判断条件以分号分隔
if a := 10; a == 10 {
}
````

## 3.2 switch使用

````go
num := 1
switch num { // 不需要括号 支持一个默认初始化语句 switch num := 1; num {
  case 1:
  	fmt.Println("1楼") 
  	break // 可以不写break，默认包含了
  case 2, 3, 4:
  	fmt.Println("2楼")
  	fallthrough // 不跳出switch语句，后面的无条件执行
  default:
  	fmt.Println("x楼")
}

// 另类写法
score := 85
switch { // 可以没有条件
  case score > 90: // case后面可以放条件
  	fmt.Println("优秀")
  case score > 80:
  	fmt.Println("良好")
  default:
  	fmt.Println("其它")
}
````

## 3.3 for使用

````go
sum := 0
for i := 1; i <= 100 i++ { // 不需要括号
	sum = sum + i  
}
````

## 3.4 range使用

````go
str := "abc"
// 通过for打印每个字符
for i := 0; i < len(str); i++ {
  fmt.Println("str[%d]=%c\n", i, str[i])
}

// 迭代打印每个元素，默认返回2个值：一个是元素的位置，一个是元素本身
for i, data := range str {
  fmt.Println("str[%d]=%c\n", i, data)
}
// 简洁写法
for i := range str {
  fmt.Println("str[%d]=%c\n", i, str[i])
}
// 等价于
for i, _ := range str {
  fmt.Println("str[%d]=%c\n", i, str[i])
}
````

## 3.5 break和continue的区别

````go
package main

import "fmt"
import "time"

func main() {
  i := 0
  
  for { // for后面不写任何东西，表示死循环
    i++
    time.Sleep(time.Second) // 延时1s
    if i == 5 {
      break // 跳出循环 如果嵌套了多个循环 跳出最近的那个内循环
      // continue
    }
    fmt.Println("i = ", i)
  }
}
````

## 3.6 goto的使用

````go
// goto可以用在任何地方，但是不能跨函数使用
fmt.Println("1")

goto End // goto是关键字 End是自定义名字

fmt.Println("2")

End:
	fmt.Println("3")
````

# 4. 函数

## 4.1 有参无返回值函数

````go
func MyFunc01(a int) {
  fmt.Println("a= ", a)
}
func MyFunc02(a int, b int) {
    fmt.Println("a= %d, b = %d", a, b)
}
// 简洁写法
func MyFunc03(a, b int) {
    fmt.Println("a= %d, b = %d", a, b)
}
// 如果每个类型都不同，需要写明类型
func MyFunc04(a int, b string, c float64) {
}
````

## 4.2 不定参数类型

````go
func MyFunc02(args ...int) {
  fmt.Println("len(args) = ", len(args))
  for i := 0; i < len(args); i++ {
    fmt.Println("args[%d] = %d\n", i, args[i])
  }
  
  // 返回2个值，第一个是下标，第二个是下标所对应的数
  for i, data := range args {
    fmt.Println("args[%d] = %d\n", i, data)
  }
}

func main() {
  MyFunc02()
  MyFunc02(1)
  MyFunc02(1, 2, 3)
}

func MyFun3(a int, args ...int) { // 不定参数一定要放在最后一个参数
  
}
````

## 4.3 不定参数传递

````go
func test(args ...int) {
  // 全部元素传递给myfunc
  // MyFunc02(args...)
  
  // 只想2个参数传递给另外一个函数使用
  MyFunc02(args[:2]...) // 传递前2个参数
  MyFunc02(args[2:]...) // 传递后2个参数
}

func main() {
  test(1, 2, 3, 4)
}
````

## 4.4 一个返回值

````go
// 无参有返回值：只有一个返回值
func myFunc01() int {
  return 666
}

// 给返回值取一个变量名，go推荐写法
func myFunc02() (result int) {
  result = 666
  return
}

func main() {
  // 无参有返回值调用
  var a int
  a = myFunc01()
  fmt.Println("a = ", a)
  
  b := myFunc01()
  fmt.Println("b = ", b)
}
````

## 4.5 多个返回值

````go
// 多个返回值
func myfunc01(int, int, int) {
  return 1, 2, 3
} 

// go官方推荐写法
func myfunc02(a int, b int, c int) { // 或者func myfunc02(a, b, c int)
  a, b, c = 111, 222, 333
  return
}

func main() {
  // 函数调用
  a, b, c := myfunc02()
  fmt.Println("a = %d, b = %d, c = %d\n", a, b, c)
}
````

## 4.6 有参有返回

````go
func MaxAndMin(a, b int) (max, min int) {
  if a > b {
    max = a
    min = b
  } else {
    max = b
    min = a
  }
  return
}

func main() {
  max, min := MaxAndMin(10, 20)
  fmt.Println("max = %d, min = %d\n", max, min)
  
  // 通过匿名函数变量丢弃某个返回值
  a, _ := MaxAndMin(10, 20)
  fmt.Println("a = %d\n", a)
}
````

 ## 4.7 函数类型

````go
// 函数也是一种数据类型，通过type给一个函数类型起名
// FuncType它是一个函数类型
type FuncType func(int, int) int // 没有函数名字，没有{}

func main() {
  var result int
  result = Add(1, 1) // 传统方式调用
  fmt.Println("result = ", result)
  
  // 声明一个函数类型的变量，变量名叫fTest
  var fTest FuncType
  fTest = Add // 是变量就可以赋值
  result = fTest(10, 20) // 等价于Add(10, 20)
}
````

## 4.8 回调函数

````go
func Add(a, b int) int {
  return a + b
}

func Minus(a, b int) int {
  return a - b
}

// 回调函数，函数有一个参数是函数类型，这个函数就是回调函数
// 计算器，可以进行四则运算
// 先有想法，后面再实现功能
func Calc(a, b int, fTest FuncType) (result int) {
  fmt.Println("Calc") 
  result = fTest(a, b) // 这个函数还没有实现
  // result = Add(a, b) // Add()必须先定义后，才能实现
  return
}

func main() {
  a := Calc(1, 1, Add)
  b := Calc(1, 1, Minus)
}
````

## 4.9 匿名函数和闭包

````go
func mamin() {
  a := 10
	str := "mike"

  // 匿名函数，没有函数名字，函数定义，还没有调用
  f1 := func() { // := 自动推导类型
    fmt.Println("a = ", a)
    fmt.Println("str = ", str)
  }
  
  f1()
  
  // ---------------------------------------------------------
  
  // 给一个函数类型起别名（这种写法比较少用）
  type FuncType func() // 函数没有参数，没有返回值
  // 声明变量
  var f2 FuncType
  f2 = f1
  f2()
  
  // ---------------------------------------------------------
  // 定义匿名函数，同时调用
  func() {
    fmt.Println("a = %d, str = %s\n", a, str)
  }() // 后面的()表示调用此匿名函数
  
  // ---------------------------------------------------------
  
  // 带参数的匿名函数
  f3 := func(i, j int) {
    fmt.Println("i = %d, j = %d\n", i, j)
  }
  f3(1, 2)
  
  // ---------------------------------------------------------
  
  // 定义匿名函数，同时调用
  func() {
    fmt.Println("a = %d, str = %s\n", a, str)
  }() // 后面的()表示调用此匿名函数
  
  // ---------------------------------------------------------
  
  // 定义匿名函数，同时调用
  func(i, j int) {
    fmt.Println("a = %d, str = %s\n", a, str)
  }(10, 20)
  
  // ---------------------------------------------------------
  
  // 匿名函数，有参有返回值
  x, y := func(i, j int) (max, min int) {
    if i > j {
      max = i
      min = j
    } else {
      max = j
      min = i
    }
    return
  }(10, 20)
  fmt.Println("x = %d, y = %d\n", x, y)
}
````

## 4.10 闭包捕获外部变量的特点

````go
func main() {
  a := 10
  str := "mike"
  
  func() {
    // 闭包以引用方式捕获外部变量
    a = 666
    str = "go"
    fmt.Println("内部：a = %d, str = %s\n", a, str) // 输出666, go
  }() // ()代表直接调用
  
  fmt.Println("外部：a = %d, str = %s\n", a, str) // 输出666, go
}
````

## 4.11 闭包的特点

````go
// 函数的返回值是一个匿名函数，返回一个函数类型
func test02() func() int {
  var x int
  return func() int {
    x++
    return x * x
  }
}

func main() {
  // 返回值为一个匿名函数，返回一个函数类型，通过f来调用返回的匿名函数，f来调用闭包函数
  // 它不关心这些捕获了的变量和常量是否已经超出了作用域
  // 所以只要闭包还在使用它，这些变量就还会存在
  f := test02()
  fmt.Println(f()) // 1
  fmt.Println(f()) // 4
  fmt.Println(f()) // 9
  fmt.Println(f()) // 16
  fmt.Println(f()) // 25
}
````

## 4.12 defer的使用

```go
func main() {
  // defer延迟使用，main函数结束前调用
	defer fmt.Println("bbbbb")
	fmt.Println("aaaaa") // 先输出aaaaa
}

```

## 4.13 多个defer执行顺序

如果一个函数中有多个defer语句，它们会以LIFO(后进先出)的顺序执行。哪怕函数或某个延迟调用发生错误，这些调用依旧会执行

```go
func test(x int) {
  fmt.Println(100 / x)
}

func main() {
  defer fmt.Println("aaa")
  defer fmt.Println("bbb")
  
  defer test(0)
  
  defer fmt.Println("ccc")
}
// 运行结果
ccc
bbb
aaa
```

## 4.14 defer和匿名函数结合使用

```go
func main() {
  a := 10
  b := 20
  
  defer func() {
    fmt.Printf("a = %d, b = %d\n", a, b)
  }() // ()代表调用此匿名函数
  
  a = 111
  b = 222
  fmt.Println("外部：a = %d, b = %d\n", a, b)
}

输出：
a = 111, b = 222
a = 111, b = 222
```

```go
func main() {
  a := 10
  b := 20
  
  defer func(a, b int) {
    fmt.Printf("a = %d, b = %d\n", a, b)
  }(a, b) // ()代表调用此匿名函数，把参数传递过去，已经先传递参数，只是没有调用
  
  a = 111
  b = 222
  fmt.Println("外部：a = %d, b = %d\n", a, b)
}

输出：
a = 111, b = 222
a = 10, b = 20
```

# 5. 获取命令行参数

```go
package main

import "fmt"
import "os"

func main() {
  // 接收用户传递的参数，都是以字符方式传递
  list := os.Args
  
  n := len(list)
  fmt.Println("n = ", n)
  
  for i := 0; i < n; i++ {
    fmt.Println("list[%d] = %s\n", i, list[i])
  }
  
  for i, data := range list {
    fmt.Println("list[%d] = %s\n", i, data)
  }
}
```

# 6. 变量

## 6.1 全局变量

```go
package main

import "fmt"

func test() {
  fmt.Println("test = a", a)
}

var a int

func main() {
  a = 10
  fmt.Println("a = ", a)
  
  test()
}

输出：
a = 10
test a = 10
```

## 6.2 不同作用域同名变量

```go
package main

import "fmt"

var a byte // 全局变量

func main() {
  var a int // 局部变量
  
  // 1、不同作用域，允许定义同名变量
  // 2、使用变量的原则，就近原则
  fmt.Println("%T\n", a)
}
```

# 7. 工程管理

1. 通过go env查看go相关的环境路径
2. 同一个目录包名必须一样，不同目录包名不一样
3. 设置GOPATH环境变量
4. 同一个目录，调用别的文件的函数，直接调用即可，无需包名引用
5. 调用不同包的，格式：包名.函数名
6. 调用别的包的函数，函数名必须大写

## 7.1 main函数和init函数

先执行导入文件的init函数，然后执行main函数所在文件的init函数，最后执行main函数

## 7.2 _操作

```go
import (
	_ "fmt" // 表示导入包，而不是直接使用包里面的函数，而是调用了该包里面的init函数
)
```

## 7.3 pkg和bin目录手动生成

配置GOBIN环境变量，指向src同级bin目录，进入src目录用go install命令即可生成

bin: 存放可执行程序

pkg: 放平台相关的库

# 8. 复合数据类型

## 8.1 指针

变量的内存和变量的地址

```go
func main() {
  var a int = 10
  // 每个变量有2层含义：变量的内存，变量的地址
  fmt.Printf("a = %d\n", a) // 变量的内存
  fmt.Printf("&a = %v\n", &a) // 变量的地址
  
  // 内存：===>教室
	// &a：===》教室外面的门牌号
  
  // 保存某个变量的地址，需要指针类型 *int 保存int的地址，**int保存*int地址
  var p *int
  p = &a
  
  *p = 666 // *p操作的不是p的内存，是p所指向的内存（就是a）
  fmt.Printf("*p = %v, a = %v\n", *p, a)
}


```

### 8.1.1 不要操作没有合法指向的内存

```go
func main() {
  var p *int
  p = nil
  fmt.Println("p = ", p)
  
  *p = 666 // err, 因为p没有合法指向
}
```

### 8.1.2 new函数的使用

```go
func main() {
  var p *int
  p = new(int)
  *p = 666
  fmt.Println("*p = ", *p)
  
  q := new(int) // 自动推导类型
  *q = 777
  fmt.Println("*q = ", *q)
}
```

### 8.1.3 指针做函数参数

```go
func swap(p1, p2 *int) {
  *p1, *p2 = *p2, *p1
}

func main() {
  a, b := 10, 20
  // 通过一个函数交换a和b的内容
  swap(&a, &b) // 地址传递
  fmt.Printf("main: a = %d, b = %d\n", a, b)
}
```

## 8.2 数组

```go
func main() {
  var id [50]int
  for i := 0; i < len(id); i++ {
    fmt.Printf("id[%d] = %d\n", i, id[i])
  }
  
  for i, data := range id {
    fmt.Printf("a[%d] = %d\n", i, data)
  }
  
  // 指定某个元素初始化 下标为2的值是10 下标为4的值是20 其它为0
  d := [5]int{2: 10, 4: 20}
  
  // 二维数组指定元素初始化
  e := [3][4]int{1: {5, 6, 7, 8}}
  
  // 支持比较，只支持 == 或 !=, 比较是不是每个元素都一样，2个数组比较，数组类型要一样
  a := [5]int{1, 2, 3, 4, 5}
  b := [5]int{1, 2, 3, 4, 5}
  c := [5]int{1, 2, 3}
  fmt.Println("a == b", a == b)
  fmt.Println("a == c", a == c)
  
  // 同类型的数组可以赋值
  var d [5]int
  d = a
  fmt.Println("d = ", d)
}
```

### 8.2.1 数组做函数参数

```go
// 数组做函数参数，它是值传递
// 实参数组的每个元素给形参数组拷贝一份
func modify(a [5]int) {
  a[0] = 666
  fmt.Println("modify a = ", a)
}

func main() {
  a := [5]int{1, 2, 3, 4, 5} // 初始化
  
  modify(a) // 数组传递过去
  fmt.Println("main: a = ", a)
}
```

### 8.2.2 数组指针做函数参数

```go
// p指向实参数组a, 它是指向数组，它是数组指针
// *p代表指针所指向的内存，就是实参a
func modify(p *[5]int) {
  (*p)[0] = 666
  fmt.Println("modify *a = ", *p)
}

func main() {
  a := [5]int{1, 2, 3, 4, 5} // 初始化
  
  modify(&a) // 地址传递
  fmt.Println("main: a = ", a)
}
```

## 8.3 随机数的使用

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	// 设置种子，只需一次
	// 如果种子参数一样，每次运行程序产生的随机数都一样
	rand.Seed(time.Now().UnixNano()) // 以当前系统时间作为种子参数
	//fmt.Println("rand = ", rand.Int()) // 随机很大的数
	fmt.Println("rand = ", rand.Intn(100)) // 限制在100以内的数
}

```

## 8.4 切片

切片并不是数组或数组指针，<font color="red">它通过内部指针和相关属性引用数组片段，以实现变长方案</font>

![image-20201101160625195](/image-20201101160625195.png)

### 8.4.1 切片的容量和长度

```go
func main() {
  a := []int{1, 2, 3, 0, 0}
  s := a[0:3:5]
  fmt.Println("s = ", s)
  fmt.Println("len(s) = ", len(s) // 长度 3-0
  fmt.Println("cap(s) = ", cap(s) // 容量 5-0
}
```

```go
package main

import "fmt"

func main() {
	// 切片
	s := []int{}
	fmt.Printf("len = %d, cap = %d\n", len(s), cap(s))

	s = append(s, 11)
	fmt.Printf("len = %d, cap = %d\n", len(s), cap(s))
}

输出：
len = 0, cap = 0
len = 1, cap = 1
```

### 8.4.2 切片的创建

```go
package main

import "fmt"

func main() {
	// 自动推导类型，同时初始化
	s1 := []int{1, 2, 3, 4}
	fmt.Println("s1 = ", s1)

	// 借助make函数，格式make(切片类型, len, cap)
	s2 := make([]int, 5, 10)
	fmt.Printf("len = %d, cap = %d\n", len(s2), cap(s2))

	// 没有指定容量，容量和长度一样
	s3 := make([]int, 5)
	fmt.Printf("len = %d, cap = %d\n", len(s3), cap(s3))
}
```

### 8.4.3 切片的截取

```go
package main

import "fmt"

func main() {
	array := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	// [low:high:max] 取下标从low开始的元素，len = high - low, cap = max - low
	s1 := array[:] // [0:len(array):len(array)] 不指定容量和长度
	fmt.Println("s1 = ", s1)
	fmt.Printf("len = %d, cap = %d\n", len(s1), cap(s1)) // 输出10 10

	// 操作某个元素，和数组操作方式一样
	data := array[1]
	fmt.Println("data = ", data)

	s2 := array[3:6:7] // a[3] a[4] a[5] len = 6 - 3 = 3, cap = 7 - 3 = 4
	fmt.Println("s2 = ", s2) // s2 =  [3 4 5]
	fmt.Printf("len = %d, cap = %d\n", len(s2), cap(s2)) // len = 3, cap = 4

	s3 := array[:6] // 从0开始，取6个元素，容量10
	fmt.Println("s3 = ", s3) // s3 =  [0 1 2 3 4 5]
	fmt.Printf("len = %d, cap = %d\n", len(s3), cap(s3)) // len = 6, cap = 10

	s4 := array[3:] // 从下标3开始 到结尾
	fmt.Println("s4 = ", s4) // s4 =  [3 4 5 6 7 8 9]
	fmt.Printf("len = %d, cap = %d\n", len(s4), cap(s4)) // len = 7, cap = 7
}
```

### 8.4.4 切片与数组的关系

```go
package main

import "fmt"

func main() {
	a := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

	// 新切片
	s1 := a[2:5] // 从a[2]开始，取3个元素
	s1[1] = 666
	fmt.Println("s1 = ", s1) // s1 =  [2 666 4]
	fmt.Println("a = ", a) // a =  [0 1 2 666 4 5 6 7 8 9]

	// 另外新切片
	s2 := s1[2:7] // 此时指针已经移动到了下标为2的位置，新的起始位置为2
	s2[2] = 777
	fmt.Println("s2 = ", s2) // s2 =  [4 5 777 7 8]
	fmt.Println("a = ", a) // a =  [0 1 2 666 4 5 777 7 8 9]
}
```

# 9. 并发

## 9.1 goroutine

goroutine说到底其实是协程，它比线程更小，十几个goroutine可能体现在底层就是五六个线程

## 9.2 主goroutine退出了，其它子协程也要跟着退出

```go
package main

import (
	"fmt"
	"time"
)

// 主协程退出了，其它子协程也要跟着退出
func main() {

	go func() {
		i := 0
		for  {
			i++
			fmt.Println("子协程 i = ", i)
			time.Sleep(time.Second)
		}
	}()

	i := 0
	for {
		i++
		fmt.Println("main i = ", i)
		time.Sleep(time.Second)

		if i == 2 {
			break
		}
	}
}
```

## 9.3 runtime包

### 9.3.1 Gosched的使用

```go
package main

import (
	"fmt"
	"runtime"
)

// 主协程退出了，其它子协程也要跟着退出
func main() {

	go func() {
		for i := 0; i < 5; i++ {
			fmt.Println("go")
		}
	}()

	// 让出时间片，先让别的协程执行完，它执行完，再回来执行主协程？
	runtime.Gosched()
	for i := 0; i < 2; i++ {
		fmt.Println("hello")
	}
}
```

### 9.3.2 Goexit终止协程

### 9.3.3 GOMAXPROCS的使用

```go
n := runtime.GOMAXPROCS(5) // 指定以多少核数运行
```

## 9.4 channel

goroutine运行在相同的地址空间，因此访问共享内存必须做好同步。goroutine奉行<font color="red">通过通信来共享内存，而不是共享内存来通信</font>

### 9.4.1 通过channel实现同步

```go
// 全局变量，创建一个channel
var ch = make(chan int)

// person1执行完后，才能到person2执行
func person1() {
  Printer("hello")
  ch <- 666 // 给管道写数据，发送
}

func person2() {
  <-ch
  // 从管道取数据，接收，如果通道没有数据他就会阻塞
  Printer("world")
}
```

### 9.4.2 通过channel实现同步和数据交互

```go
package main

import (
	"fmt"
	"time"
)

// 主协程退出了，其它子协程也要跟着退出
func main() {

	ch := make(chan string)
	defer fmt.Println("主协程也结束")

	go func() {
		defer fmt.Println("子协程调用完毕")

		for i := 0; i < 2; i++ {
			fmt.Println("子协程 i = ", i)
			time.Sleep(time.Second)
		}

		ch <- "我是子协程，工作完毕"
	}()

	str := <-ch // 没有数据前，阻塞
	fmt.Println("str = ", str)
}
```

### 9.4.3 关闭channel

```go
func main() {
  ch := make(chan int) // 创建一个无缓冲的channel
  
  // 新建一个goroutine
  go func() {
    for i:= 0; i < 5; i++ {
      ch <- i
    }
    // 不需要再写数据时，关闭channel
    close(ch) // 关闭后无法再发送数据
  }()
  
  for {
    // 如果ok为true，说明管道没有关闭
    if num, ok := <-ch; ok == true {
      fmt.Println("num = ", num)
    } else {
      break
    }
  }
  // 另一种写法
  for num := range ch {
    fmt.Println("num = ", num)
  }
}
```

