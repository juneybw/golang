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
