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

```go
定义:
var arr [n]type——————在[n]type 中，n 表示数组的长度，type 表示存储元素的类型

简写:
arr1 := [4]int{1,2,3,4}

arr2 := [...]int{9，9，9}——————————go会自动根据元素个数来计算数组长度

多维数组:
arr3 := [2][3]int{{1,2,3},{4,5,6}}

操作:
（1）两个数组之间的比较需要借助于reflect.DeepEqual(arr1,arr2)来进行比较
（2）数组之间的赋值是值的赋值，即当把一个数组作为参数传入函数的时候，传入的实际上是
该数组的副本，而不是它的指针
（3）值得注意的是，由于数组长度及类型参与了数组的构建，所以二者只要存在其一不一致的两个数组，比较时均返回false,只有数组长度和类型一模一样时才会返回true
```

###### slice

```go
概念:引用类型。slice总是指向底层array

定义:
var arr []type——————type表示存储元素的类型（和声明array一样，只是少了长度）

结构:
{	
    指针
	当前长度
 	最大长度
}

操作:
（1）可以从现有数组中截取
    arr := [5]int{1,2,3,4,5}
    s := arr[2:5]
（2）便捷操作要领
	slice的默认开始位置是 0，ar[:n]等价于 ar[0:n]
	slice的第二个序列默认是数组的长度，ar[n:]等价于 ar[n:len(ar)]
	如果从一个数组里面直接获取slice，可以这样 ar[:]，因为默认第一个序列是 0，第二个是数组的长度，即等价于 ar[0:len(ar)]

内置函数:
	len获取slice的长度
	cap获取slice的最大容量
	append向slice里面追加一个或者多个元素，然后返回一个和slice一样类型的slice
	copy函数copy从源slice的src中复制元素到目标dst，并且返回复制的元素的个数
注意:
	append函数会改变slice所引用的数组的内容，从而影响到引用同一数组的其它slice。但当slice中没有剩余空间（即(cap-len) == 0）时，此时将动态分配新的数组空间。返回的slice数组指针将指向这个空间，而原数组的内容将保持不变；其它引用此数组的slice则不受影响
```

###### map

```go
概念：
	键值对

定义并初始化：
	方法一:
	m := make(map[keyType] valueType)
	添加元素：
	m[keyType] = valueType
	
	方法二:
	var n map[keyType] valueType
	n = make(map[keyType]valueType)
	n[keyType] = valueType
	
	方法三:
	k := map[keyType] valueType{keyType:valueType,...}

需要了解:
	map是无序的，每次打印出来的map都会不一样，它不能通过index获取，而必须
通过 key 获取
	map长度不固定，和slice一样，也是一种引用类型
	内置的len函数同样适用于map，返回map拥有的key的数量
	map的值可以很方便的修改，通过 变量名[key]=val 可以很容易的把key为新的val
	map的初始化可以通过key:val的方式初始化值，同时map内置有判断是否存在key的方
式

删除元素:
	delete(变量名, key) 
```

###### new和make区别

```
暂时理解不透彻，后续补进
```

##### 流程控制和函数

###### if

```go
规范:
	go语言中if条件判断语句不需要使用括号
	条件判断语句里面允许声明一个变量，这个变量的作用域只能在该条件逻辑块内，其他区域无作用

例子:

if x > 10 {
    fmt.Println("x is greater than 10")
} else {
    fmt.Println("x is less than 10")
}


if x := computedValue(); x > 10 {
    fmt.Println("x is greater than 10")
} else {
    fmt.Println("x is less than 10")
}
//这个地方如果这样调用就编译出错了，因为 x 是条件里面的变量
fmt.Println(x)
```

###### goto

```go
 概念:
 使用goto跳转到必须在当前函数内定义的标签，标签名是大小写敏感的
 
 例子:
 func myFunc() {
 	i := 0
 Here: //这行的第一个词，以冒号结束作为标签
	println(i)
	i++
	goto Here //跳转到 Here 去
}
```

###### for

```go
语法：
for expression1; expression2; expression3 {
	//...
}
解析：
1.expression1、expression2和expression3均为表达式
2.其中expression1和expression3是变量声明或者函数调用返回值之类的
3.expression2是用来条件判断
4.expression1在循环开始之前调用，expression3 在每轮循环结束之时调用

操作:
1.循环里面有两个关键操作 break 和 continue

	break操作是跳出当前循环，continue是跳过本次循环(当嵌套过深的时候，break 可以配合标签使用，即跳转至标签所指定的位置)

2.break和continue还可以跟着标号，用来跳到多重循环中的外层循环

3.for配合range可以用于读取slice和map的数据
```

