#method
##定义
* receiver遵循正常的参数规则，可以传值或者地址，地址可以修改struct中的值
* 可以为每个类型增加方法，struct也属于一种类型，所以可以增加方法
* 只能对相同包中的类型绑定方法
* 底层类型的方法不会带到外部类型如type TZ int，int中的方法不适合tz类型，所以需要类型转换
* 私有字段的是package为级别

##method value & method expression
```go
a.print()// method value
(*TZ).print(&a)// method expression
```

