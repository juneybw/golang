#interface
##定义
* 一个或多个方法的声明的集合，没有实现
* 某类型无需显示说明实现了xx接口
* ???遇到一个问题???结构的方法名和变量名不能重复？报错？如下
```go
type USB interface {
	Name() string
	Connect()
}

type PhoneConnect struct {
	name string//这里如果name的N大写，与PhoneConnect的Name方法重名，编译报错！
}

func (pc PhoneConnect) Name() string {
	return pc.name
}

func (pc PhoneConnect) Connect() {
	fmt.Println("connect : " + pc.name)
}

func main() {
	var a USB
	a = PhoneConnect{"PhoneConnect"}
	a.Connect()
}
```
