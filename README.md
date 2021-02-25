# GoLang

GoLang学习笔记

##### 基础语法

###### 变量

1.1go使用var关键字定义变量，将变量类型放于变量名后面

```go
var variableName type

例子：var name string
```

1.2定义变量时可以定义多个一起，以及初始化他们

```go
定义多个变量
var vname1,vname2,vname3 type
定义单个变量并初始化
var vname = value
定义多个变量并初始化(变量类型一致时可以省略类型)
var vname1,vname2,vname3 type = value1,value2,value3
省略变量类型
var vname1,vname2,vname3 = value1,value2,value3 
终极简化——用:=符号代替var和type
vname := value
```

**限制：**在函数外部使用var定义变量会编译不通过，因此通常用var定义全局变量

1.3特殊点

```go
(1) _下划线为一个特殊的变量名————————任何赋予它的值将被丢弃掉
(2) go语言中对于已声明但未使用的变量会在编译期间报错
```

###### 常量

​	在程序编译阶段就确定下来的值，而程序在运行时则无法改变该值  

2.1go语言用const关键字定义常量，常量可定义为数值、布尔值或字符串等类型 

```go
const constantName = value
例子：
const a = 2021
const b = true
const c = "golang"
```

###### 内置基础类型

3.1布尔型

```go
go语言中布尔型用bool表示，值为true或false，默认为false
```

3.2数值类型

```
（1）整数类型分：有无符号和带符号两种。
	go同时支持int和uint，这两种类型的长度相同，但具体长度取决于不同编译器的实现。
	当前的gcc和gccgo编译器在32位和64位平台上都使用32位来表示int和uint，但未来在 64 位平台上可能增加到 64 位。
	go里面也有直接定义好位数的类型：int8, int16, int32, int64和uint8, uint16, uint32, uint64。
	另外存在rune代表int32的别称，byte代表uint8的别称
（2）复数————默认类型是complex128 64位实数+64位虚数）
	形式：RE + IMi （RE是实数部分，IM是虚数部分，而最后的i是虚数单位）
```

注意点：变量之间不允许互相赋值或操作，不然会在编译时引起编译器报错  

3.3字符串

```go
（1）go中的字符串都是采用UTF-8字符集编码。
（2）字符串是用一对双引号（""）或反引号（` `）括起来定义，它的类型是string
```

注意点： 1.go中字符串不可变，但可进行切片操作

​				2.可以使用+操作符来连接两个字符串

3.4错误类型

```go
go内置有一个error类型，专门用来处理错误信息，go的package里面专门用一个包errors来处理错误
```

技巧：

```go
定义多个常量或变量时可以进行分组定义

例子1:

package main

import "fmt"

func main() {

	const (
		a = iota
		b
		c = iota
		d
	)
	
	var(
		e = 20
		f = "golang"
		g = 200.1
	)


	fmt.Print("\n")
	fmt.Print(a)
	fmt.Print("\n")
	fmt.Print(b)
	fmt.Print("\n")
	fmt.Print(c)
	fmt.Print("\n")
	fmt.Print(d)

	fmt.Print("\n")
	fmt.Print(e)
	fmt.Print("\n")
	fmt.Print(f)
	fmt.Print("\n")
	fmt.Print(g)

}
输出：
0
1
2
3
20
golang
200.1


例子2:

package main

import "fmt"

func main() {
	const (
		a  = 2
		b
		c = iota
		d
	)
	var(
		e = 20
		f = "golang"
		g = 200.1
	)


	fmt.Print("\n")
	fmt.Print(a)
	fmt.Print("\n")
	fmt.Print(b)
	fmt.Print("\n")
	fmt.Print(c)
	fmt.Print("\n")
	fmt.Print(d)

	fmt.Print("\n")
	fmt.Print(e)
	fmt.Print("\n")
	fmt.Print(f)
	fmt.Print("\n")
	fmt.Print(g)

}
输出:
2
2
2
3
20
golang
200.1

注意点:
（1）当分组定义变量或者常量时，第一个常量或变量默认为0索引位上的值，直到遇到显式赋值或者iota，才会改变该位置上的值，否则后一个变量或者常量的值均为前一个索引位上的值
（2）当遇到iota时因为每次调用iota会进行自增，所以尽管后一个的值仍旧等于前一位的值，但是会进行+1处理
```

###### go语言默认设计规则

```go
（1）大写字母开头的变量是可导出的，也就是其它包可以读取的，是公用变量；小写字母开头的就是不可导出的，是私有变量。
（2）大写字母开头的函数也是一样，相当于class中的带public关键词的公有函数；小写字母开头的就是有private关键词的私有函数
```

##### 容器

###### array
