##定义

###普通反射提取字段和方法
```go
import (
	"fmt"
	"reflect"
)

type User struct {
	Id   int
	Name string
	Age  int
}

func (u User) Hello() {
	fmt.Println("hello world")
}

func main() {
	u := User{1, "Joe", 12}
	Info(u)//值拷贝传递
}

func Info(o interface{}) {
	t := reflect.TypeOf(o)          //获取传入的接口类型
	fmt.Println("Name: ", t.Name()) //打印类型

	if k := t.Kind(); k!=reflect.Struct {//判断o的类型是不是struct
		fmt.Println("wrong")
		return
	}

	v := reflect.ValueOf(o) //
	fmt.Println("Fileds : ")

	for i := 0; i < t.NumField(); i++ {
		f := t.Field(i)
		val := v.Field(i).Interface()
		fmt.Printf("%6s : %v = %v\n", f.Name, f.Type, val)
	}

	for i := 0; i < t.NumMethod(); i++ { //打印方法名
		m := t.Method(i)
		fmt.Printf("%6s : %v", m.Name, m.Type)
	}
}
```
输出
```go
	Name:  User
	Fileds : 
	    Id : int = 1
	  Name : string = Joe
	   Age : int = 12
	 Hello : func(main.User)
```
###匿名字段反射
```go
type User struct {
	Id   int
	Name string
	Age  int
}
type Manager struct {
	User
	Title string
}
func main() {
	m := Manager{User: User{1, "Joe", 12}, Title: "manager"}
	t := reflect.TypeOf(m)
	fmt.Printf("%#v\n", t.FieldByIndex([]int{0, 1}))//传入slice获取User的Name字段
}
```

###通过反射修改值
```go
x := 123
v := reflect.ValueOf(&x)
v.Elem().SetInt(999)
fmt.Println(x)
```
修改类型中的字段
```go
type User struct {
	Id   int
	Name string
	Age  int
}

func main() {
	u := User{1, "okok", 12}
	SetU(&u)
	fmt.Println(u)
}

func SetU(o interface{}) {
	v := reflect.ValueOf(o)

	if v.Kind() == reflect.Ptr && !v.Elem().CanSet() {
		fmt.Println("xxx")
		return
	} else {
		v = v.Elem()
	}
	f := v.FieldByName("Name")//判断字段名是否在类型中
	if !f.IsValid() {
		fmt.Println("Ban")
		return
	}
	if f.Kind() == reflect.String {
		f.SetString("lalala")
	}
}
```
###反射动态调用方法
```go
type User struct {
	Id   int
	Name string
	Age  int
}

func (u User) Hello(name string) {
	fmt.Println("hello ", name, ",my name is ", u.Name)
}

func main() {
	u := User{1, "joe", 12}
	v := reflect.ValueOf(u)
	mv := v.MethodByName("Hello")
	args := []reflect.Value{reflect.ValueOf("Mike")}
	mv.Call(args)
}
```
