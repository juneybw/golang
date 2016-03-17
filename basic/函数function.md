# golang
golang learn

#Map
#####对调key value
```go
import (
	"fmt"
)

func main() {
	m1 := map[int]string{1: "a", 2: "b", 3: "d"}
	fmt.Println(m1)
	m2 := make(map[string]int)

	for k, v := range m1 {
		m2[v] = k
	}
	fmt.Println(m2)
}
```

#函数function
##命名返回值函数
```go
func A() (a, b, c int) {
	a, b, c = 1, 2, 3//在定义返回值时已经定义了，所以在这里不需要:=
	return
}
```
##不定长变参函数
* 必须作为参数列表的最后一个参数
* 值拷贝传递，任何操作不影响原变量，如果直接传入slice则是传地址，就会影响
```go
func A(a ...int) {
	fmt.Println(a)
}
```
##匿名函数
* 必须放在一个外层函数中，不能直接作为最外层函数
```go
func main() {
	a := func() {
		fmt.Println("func a")
	}
	a()
}
```
##闭包
* 函数引用了环境中的变量，如closure返回的函数引用了x，x是依靠地址的，每次调用这个函数都指向了同一个x，都是创建a时的x地址
```go
func main() {
	a := closure(10)
	fmt.Println(a(2))
}
func closure(x int) func(int) int {
	return func(y int) int {
		return x + y
	}
}
```
##defer
* 后定义先调用
* 下列代码三次输出都是3，实际上用了闭包，因为defer函数时，里面的变量i是地址拷贝引用
```go
func main() {
	for i := 0; i < 3; i++ {
		defer func() {
			fmt.Println(i)
		}()
	}
}
```
* 传说中的异常处理 panic和recover ，recover必须在defer中定义才有用
```go
func main() {
	A()
	B()
	C()
}

func A() {
	fmt.Println("func A")
	//结果是A recover in B C
}

func B() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("recover in B")
		}
	}()
	panic("Panic in B")
}

func C() {
	fmt.Println("func C")
}
```
##匿名函数 defer 闭包运用
```go
func main() {
	var fs = [4]func(){}
	for i := 0; i < 4; i++ {
		defer fmt.Println("defer i = ", i)
		defer func() { fmt.Println("defer_closure i = ", i) }()
		fs[i] = func() {
			fmt.Println("closure i = ", i)
		}
	}

	for _, f := range fs {
		f()
	}
}
```
* 输出为
	closure i =  4
	closure i =  4
	closure i =  4
	closure i =  4
	defer_closure i =  4
	defer i =  3
	defer_closure i =  4
	defer i =  2
	defer_closure i =  4
	defer i =  1
	defer_closure i =  4
	defer i =  0
因为从外侧直接抓来的变量是抓的地址，而第一个defer用的是值拷贝


