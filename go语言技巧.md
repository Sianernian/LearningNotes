[TOC]

#### go build -ldflags "-H=windowsgui"    xxxx.go   后台 damon运行

#### 多个if 语句可以使用 switch 语句折叠

```go
// NOT BAD
if foo() {
    // ...
} else if bar == baz {
    // ...
} else {
    // ...
}

// BETTER
switch {
case foo():
    // ...
case bar == baz:
    // ...
default:
    // ...
}
```

#### 传递信号可以使用 chan struct{}

#### 在 Go 里面要小心使用 `range`:

- `for i := range a` and `for i, v := range &a` ，都不是 `a` 的副本
- 但是 `for i, v := range a` 里面的就是 `a` 的副本

#### 从map中取出一个不存在的key 不会panic,得到的是一个0值

``` go
type Point struct {
  X, Y float64
  _    struct{} // to prevent unkeyed literals
}
/*
  对于 Point {X：1，Y：1} 都可以，但是对于 Point {1,1} 则会出现编译错误：
 	./file.go:1:11: too few values in Point literal 
*/

 不要在循环中使用 defer，否则会导致内存泄露
因为这些 defer 会不断地填满你的栈（内存）
```



#### 检查 map 是否含有某一元素

```go
 x := map[string]string{"one":"a","two":"","three":"c"}
    if _,ok := x["two"]; !ok {
        fmt.Println("no entry")
    }
```



#### 切片和数组共用同一个底层数组，slice使用 [:] 截取时在修改值时，底层数组也会改变值

```
当使用 s1[1:] 获得切片 s2，和 s1 共享同一个底层数组，这会导致 s2[1] = 4 语句影响 s1。

而 append 操作会导致底层数组扩容，生成新的数组，因此追加数据后的 s2 不会影响 s1。
```

#### String()方法，默认输出时会自动调用String（）方法，实现字符串的打印

如果使用指针则不会自动调用String()方法  eg:  func (d *Direction)String()string{}

为什么使用指针方式定义String()方法就不行了？  

可能如果传入的是指针类型，那么String方法可能修改原来的数据，go语言会认为此时的String()方法并不一定是单纯的输出字符串，因此如果再自动调用该方法就不合适了。而使用传值方式，数据不会进行改变，因此可以自动调用该方法。这种机制使得编程语言更加安全、可靠。

```go
type Direction int

const (
    North Direction = iota
    East
    South
    West
)

func (d Direction) String() string {
    return [...]string{"North", "East", "South", "West"}[d]
}

func main() {
    fmt.Println(South)
} // 打印结果为 south
```

#### 作用域易错点   

[eg]: https://tonybai.com/2015/01/13/a-hole-about-variable-scope-in-golang/	"原文讲解"

https://tonybai.com/2015/01/13/a-hole-about-variable-scope-in-golang/

```go
// eg:
var p *int

func foo()(*int,error){
	var i int=5
	return &i,nil
}

func bar(){
	fmt.Println(*p)
}

func main(){
	p,err:=foo()
	if err !=nil{
		fmt.Println(err)
		return
	}
	bar()
	fmt.Println(*p)
}
// 运行painc  无效的控制空指针      bar()函数的无效空指针，如果去掉 * 则输出的结果为 nil， mina函数不打印 fmt.Println(*p)
// 修改
func main(){
	var err error
	p,err=foo()
	if err!=nil{
		.....
		....
	}
	.....
	.....
}
// 变量作用域。问题出在操作符:=，对于使用:=定义的变量，如果新变量与同名已定义的变量不在同一个作用域中，那么 Go 会新定义这个变量。对于本例来说，main() 函数里的 p 是新定义的变量，会遮住全局变量 p，导致执行到bar()时程序，全局变量 p 依然还是 nil，程序随即 Crash。
```

#### for range 使用短变量声明(:=)的形式迭代变量，需要注意的是，变量 i、v 在每次循环体中都会被重用，而不是重新声明。

```go
func main() {

    var m = [...]int{1, 2, 3}

    for i, v := range m {
        go func() {
            fmt.Println(i, v)
        }()
    }

    time.Sleep(time.Second * 3)
} 
// 输出 2 3  2 3    2  3 
//修改 闭包引用：
go func(i,v int){
	fmt.Println(i,v)
}(i,v)
```

#### channel 遍历数据时

```go
//无限循环从通道中读取数据
	for i := range ch {
		fmt.Println(i)
	}

// 等价于
//无限循环从通道中读取数据
	for {
		i, ok := <-ch
		if !ok {
			break
		}
		fmt.Println(i)
	}
```

##### 知识点：常量。常量不同于变量的在运行期分配内存，常量通常会被编译器在预处理阶段直接展开，作为指令数据使用，所以常量无法寻址。	

#### 输出321。第一次循环，写操作已经准备好，执行 o(3)，输出 3；第二次，读操作准备好，执行 o(2)，输出 2 并将 c 赋值为 nil；第三次，由于 c 为 nil，走的是 default 分支，输出 1。

```go
var o = fmt.Print

func main() {
    c := make(chan int, 1)
    for range [3]struct{}{} {
        select {
        default:
            o(1)
        case <-c:
            o(2)
            c = nil
        case c <- 1:
            o(3)
        }
    }
}
```

