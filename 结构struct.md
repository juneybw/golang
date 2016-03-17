#struct 
##定义
* type <Name> struct{}
* 是一种类型，在go语言中代替class，但是又没有class的功能，没有继承
* 直接传递的时候也是值拷贝
* 一般直接将变量定义为struct的指针，就是在定义是加一个&

##匿名结构
```go
a := &struct {
		Name string
		Age  int
	}{
		Name: "joe",
		Age:  19,
	}
	fmt.Println(a)//{joe 19}
```
* 结构中的匿名结构
```go
type Person struct {
	Name    string
	Age     int
	Contact struct {//注意这里Contact不是结构名称，而是Person结构中的字段名称
		Phone, city string
	}
}

func main() {
	a := Person{Name: "joe", Age: 19}
	a.Contact.Phone = "1111"
	a.Contact.city = "Beijing"
	fmt.Println(a)
}
```
##匿名字段
* 匿名字段中，将会把类型名作为字段的名称
```go
type Person struct {
	string
	int
}

func main() {
	a := Person{"joe", 19}//这里赋值字段时，必须和定义时候的顺序一致
	fmt.Println(a)
}
```
##结构之间比较
* 结构命一致，内容一致，如果做a==b比较，返回 true

##嵌入结构，go中的组合
* 需要注意的是嵌入匿名结构的名称会自动变为结构名，所以赋值的时候要使用human：human{}的形式
* 并且在使用sex字段时，可以直接a.Sex这样使用
* 如果Teacher中也有Sex字段，则a.Sex指向外层中的Sex，而不是human中的Sex
```go
type human struct {
	Sex int
}

type Teacher struct {
	human
	Name string
	Age  int
}

type Student struct {
	human
	Name string
	Age  int
}

func main() {
	a := Teacher{Name: "joe", Age: 10, human: human{Sex: 1}}
	b := Student{Name: "joe", Age: 10, human: human{Sex: 0}}
	a.Sex=11
	fmt.Println(a)
	fmt.Println(b)
}
```
