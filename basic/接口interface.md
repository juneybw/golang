#interface
##定义
* 一个或多个方法的声明的集合，没有实现
* 某类型无需显示说明实现了xx接口
* ???遇到一个问题???结构的方法名和变量名不能重复？报错？如下

##嵌入接口如嵌入结构
如代码

##类型判断
代码中Disconnect方法没法获取usb接口指向的PhoneConnect中的name字段，这里需要用到ok pattern判断usb所指向类型
```go
package main

import "fmt"

type USB interface {
	Name() string
	Connector //这里就是嵌入结构了
}

type Connector interface {
	Connect()
}
type PhoneConnect struct {
	name string //这里如果name的N大写，与PhoneConnect的Name方法重名，编译报错！
}

func (pc PhoneConnect) Name() string {
	return pc.name
}

func (pc PhoneConnect) Connect() {
	fmt.Println("connect : " + pc.name)
}

func main() {
	var a USB
	a = PhoneConnect{name: "PhoneConnect"}
	a.Connect()
	Disconnect(a)
}

func Disconnect(usb USB) {
	if pc, ok := usb.(PhoneConnect); ok { //使用ok pattern判断
		fmt.Println("Disconnect:" + pc.name)
		return
	}
	fmt.Println("Disconnect: unkown device.")
}
```

##空接口
* 如其他语言中的定级类型Object，是所有类型的接口
```go
type empty interface{}
```
* 改一下上述代码中的Disconnect方法，让他适应更多的类型
```go
func Disconnect(usb interface{}) {
	switch v := usb.(type) {
	case PhoneConnect:
		fmt.Println("Disconnect:" + v.name)
	default:
		fmt.Println("Disconnect: unkown device.")
	}
}
```

##接口之间的转换 
* 超集接口可以转换为子接口，就是包含a接口中所有方法的b接口可以转换为a接口，如复制品中的代码

##复制品
* 接口拿到的对象是原对象的复制品，修改不会影响原来的对象
```go
pc := PhoneConnect{name: "PhoneConnect"}
	var a Connector
	a = Connector(pc)
	a.Connect()
	pc.name = "pc"//这里将pc的name字段修改
	a.Connect()//打印出来仍然是PhoneConnect，而不是修改后的pc，所以接口转换拿到的是原pc对象的拷贝！
```

##接口调用不会做receiver的自动转换
* 就是说使用接口调用方法，如果方法的receiver要求是指针，就得使用指针调用，指针调用可以调用receiver是指针或者不是指针的方法
