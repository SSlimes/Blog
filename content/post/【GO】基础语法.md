---
title: 【GO】基础语法
date: 2025-09-06
categories:
  - 安全开发
image: images/50.webp
---
# 0x01 认识函数
语法最基础函数：
```go
package main  
  
import "fmt"  
  
func main() {  
    fmt.Println("hello world")  
}
```
## 1. package
```
包是Go语⾔的基本组成单元，通常使⽤单个的⼩写单词命名，⼀个 Go 程序本质上就是⼀组包的集合。所有Go代码都有⾃⼰⾪属的包。main 包在Go中是⼀个特殊的包,整个Go程序中仅允许存在⼀个名为 main 的包。
```
函数：
```
Go 语⾔中，函数名的⾸字⺟决定了其可⻅性。如果函数名的⾸字⺟⼤写，那么该函数是导出的，可以在包外部使⽤；如果⾸字⺟⼩写，那么该函数是未导出的，只能在包内部使⽤。
```
# 0x02 基本类型
### 字符串 Strings
**单引号跟双引号的区别**
单引号:
```
⽤途：⽤于表示字符（rune）类型。
类型：字符类型在Go中是rune，它是int32的别名，可以表示⼀个Unicode码点
```
双引号:
```
⽤途：⽤于表示字符串（string）类型。
类型：字符串类型在Go中是string，它是⼀个字符序列，可以包含零个或多个字符
```
### 数字 Numbers
```
int
uint
uint32
```
不做太多解释。
### 布尔值 Booleans
```go
package main  
  
import "fmt"  
  
func main() {  
    fmt.Println(true && true)  
    fmt.Println(true && false)  
    fmt.Println(true || true)  
    fmt.Println(true || false)  
    fmt.Println(!true)  
}
输出：
true
false
true
true
false
```
# 0x03 变量声明
变量声明：
```go
var s1 string  
s1 = "hello"
```
简短声明：
```go
s2 := "hell0"
```
一次声明多个变量：
```go
var s3,s4 string  
s5,s6 :="G","o"
```
匿名变量：
```go
_, s7 := "1", "2"
```
# 0x04 注释
```
单行注释：// 
多行注释：/*  */
```
# 0x05 fmt.Println与print
使⽤场景:
```
fmt.Println:更常⽤于实际开发中，功能更强⼤，输出格式更灵活。
println:通常⽤于简单的调试或测试。
```
# 0x06 运算符
**算术运算符:**
+（加法）
```
a := 5 + 3
```
-（减法）
```
a := 5 - 3
```
乘法
```
a := 5 * 3
```
/（除法）
```
a := 5 * 3
```
%（取模）
```
a := 5 % 3
```
**赋值运算符:**
=（赋值）
```
a = 5 // 将5赋值给a
```
+=（加赋值）
```
a := 5
a += 3 // 等价于a = a + 3，结果是8
```
-=（减赋值）
```
a := 5
a -= 3 // 等价于a = a - 3，结果是2
```
`*=` (乘赋值)
```
a := 5
a *= 3 // 等价于a = a * 3，结果是15
```
/= (除赋值)
```
a := 6
a /= 3 // 等价于a = a / 3，结果是2
```
%= (取模赋值)
```
a := 5
a %= 3 // 等价于a = a % 3，结果是2
```
**关系运算符**
== （等于）
```
a := 5
b := 5
fmt.Println(a == b) // 结果是true
```
!=（不等于）
```
a := 5
b := 3
fmt.Println(a != b) // 结果是true
```
<（⼩于）
```
a := 3
b := 5
fmt.Println(a < b) // 结果是true
```
<=（⼩于等于）
```
a := 5
b := 5
fmt.Println(a <= b) // 结果是true
```
'>'（⼤于）
```
a := 5
b := 3
fmt.Println(a > b) // 结果是true
```
（⼤于等于）
```
a := 5
b := 5
fmt.Println(a >= b) // 结果是true
```
**逻辑运算符**
&&（逻辑与）
```
a := true
b := false
fmt.Println(a && b) // 结果是false
```
||（逻辑或）
```
a := true
b := false
fmt.Println(a || b) // 结果是true
```
!（逻辑⾮）
```
a := true
fmt.Println(!a) // 结果是false
```
**位运算符**
&（按位与）
```
a := 5 // 0101
b := 3 // 0011
fmt.Println(a & b) // 结果是1（0001）
```
|（按位或）
```
a := 5 // 0101
b := 3 // 0011
fmt.Println(a | b) // 结果是7（0111）
```
^（按位异或）
```
如果两个数的对应位只有⼀个是 1，那么结果位为 1。
如果两个数的对应位都是 1 或都是 0，那么结果位为 0。
a := 5 // 0101
b := 3 // 0011
fmt.Println(a ^ b) // 结果是6（0110）
```
<<（左移） 保留0
```
a := 1 // 0001
fmt.Println(a << 2) // 结果是4（0100）
```
》》(右移）不保留0
```
a := 4 // 0100
fmt.Println(a >> 2) // 结果是1（0001）
```
**复合赋值运算符**
&=（按位与赋值）
```
a := 5 // 0101
a &= 3 // 结果是1（0001）
```
|=（按位或赋值）
```
a := 5 // 0101
a |= 3 // 结果是7（0111）
```
^=（按位异或赋值）
```
a := 5 // 0101
a ^= 3 // 结果是6（0110）
```
**其它运算符**
++（⾃增）
```
a := 5
a++ // 结果是6
```
--（⾃减）
```
a := 5
a-- // 结果是4
```
:=（短变量声明）
```
a := 5 // 声明并初始化变量a
```
<-（通道操作符）
```go
ch := make(chan int)
go func() {
ch <- 5 // 发送值到通道
}()
a := <-ch // 从通道接收值
fmt.Println(a) // 结果是5
```
**分组和分隔符**
( 和 )（⼩括号）
```
a := (5 + 3) * 2 // ⼩括号⽤于改变运算优先级
```
[ 和 ]（中括号）
```
arr := [3]int{1, 2, 3} // 定义数组
fmt.Println(arr[0]) // 访问数组元素，结果是1
```
{ 和 }（⼤括号）
```go
func main() {

fmt.Println("Hello, World!")

}
```
,（逗号）
```
a, b := 1, 2 // 多变量声明
```
;（分号）
```go
// Go语⾔中分号通常是隐式的，但可以显式使⽤
a := 5; b := 3; 
fmt.Println(a + b)
```
...（省略号）
```go
func sum(nums ...int) int {
total := 0
for _, num := range nums {
total += num
}
return total
}
fmt.Println(sum(1, 2, 3)) // 结果是6
```
.（点号）
```go
fmt.Println("Hello, World!") // 调⽤包中的函数
```
**快捷键**
```
Ctrl+B 转到声明
Ctrl+p 查看这个函数可以接收什么内容
.var 快速⽣成变量名
shift+空格 下⼀⾏
```
小坑：
```
创建项⽬记得⽤英⽂，不能⽤中⽂创建项⽬

⽆法相互调⽤，看看是不是mod包错了
```
# 0x07 常量声明
## 常量定义
```
常量使⽤ const 关键字定义。
常量的值在编译时就确定了，不能在运⾏时改变。
```
## 常量类型
```
常量可以是布尔型、数字型（整数、浮点数、复数）、字符串型。
常量的类型可以是显式声明的，也可以是隐式的。
```
例如：
```go
package main  
  
import "fmt"  
  
func main() {  
    var s1 int = 10 //变量举例，可以动态改变
    fmt.Println(s1) //输出10  
    s1 = 20  
    fmt.Println(s1) //输出20  
  
    const s2 = 10   //常量举例，不可以被改变
    fmt.Println(s2) //输出10  
    s2 = 20         //不能被修改，这里运行错误  
    fmt.Println(s2)  
}
```
## 多个常量的分组声明
可以使用consts进行声明：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250527141633.png)
多行进行声明：
```
const (  
    q int    = 10  
    w string = "hello"  
)
```
var也可以进行多行声明：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250527141921.png)
## ⽆类型常量声明
⽆类型常量在需要时会被隐式转换为适当的类型
```go
func main() {  
    const pi = 3.1415926  
}
```
## iota 枚举
```
iota 是⼀个常量⽣成器，⽤于⽣成⼀系列相关值。
iota 在每个 const 声明块中从 0 开始，逐⾏递增。
```
示例代码：
```go
package main  
import "fmt"  
const (  
    pi       = 3.1415926  
    Language = "Go"  
    IsTrue   = true  
)  
const (  
    Zero = iota  
    One  
    Two
)  
func main() {  
    fmt.Println("Pi:", pi)  
    fmt.Println("Language:", Language)  
    fmt.Println("IsTrue:", IsTrue)  
    fmt.Println("Zero:", Zero)  
    fmt.Println("One:", One)  
    fmt.Println("Two:", Two)  
}
输出：
Pi: 3.1415926
Language: Go
IsTrue: true
Zero: 0
One: 1
Two: 2
```
## 变量与常量的区别
```
定义⽅式：常量使⽤ const，变量使⽤ var 或 :=。
值的修改：常量不可修改，变量可修改。
内存分配：常量在编译时确定，不占⽤运⾏时内存；变量在运⾏时分配内存。
使⽤场景：常量⽤于定义不会改变的值，变量⽤于定义可能会改变的值。
```
# 0x08 数组
#### 1.什么是数组？
在 Go 中，数组是具有固定⻓度且元素类型相同的集合。数组的定义语法如下：
```go
var arrayName [length]elementType
var numbers [5]int
```
#### 2.初始化数组
数组可以在声明时进⾏初始化
```go
func main() {  
    var number = [5]int{1, 2, 3, 4, 5}  
    fmt.Println(number)  
}
输出：[1 2 3 4 5]
```
#### 3.简短声明⽅式
```go
func main() {  
    numbers := [5]int{1, 2, 3, 4, 5}  
    fmt.Println(numbers)  
}
输出：[1 2 3 4 5]
```
#### 4.使⽤省略让编译器⾃动计算数组⻓度
```go
func main() {  
    numbers := [...]int{1, 2, 3, 4, 5}  
    fmt.Println(numbers)  
}
输出：[1 2 3 4 5]
```
#### 5.访问数组元素
```go
func main() {  
    numbers := [...]int{1, 2, 3, 4, 5}  
    fmt.Println(numbers[0]) // 输出第⼀个元素  
}
输出：1
```
#### 6.修改数组中的元素：
```go
func main() {  
    numbers := [...]int{1, 2, 3, 4, 5}  
    numbers[1] = 10 // 修改第⼆个元素的值  
    fmt.Println(numbers)  
}
输出：[1 10 3 4 5]
```
#### 7.数组⻓度
```go
func main() {  
    numbers := [...]int{1, 2, 3, 4, 5}  
    fmt.Println(len(numbers)) // 输出数组的⻓度  
}
输出：5
```
#### 8.值类型
数组是值类型，这意味着当数组被赋值给另⼀个数组或作为参数传递时，会进⾏值拷⻉
```go
func main() {  
    a := [3]int{1, 2, 3}  
    b := a  
    b[0] = 10  
    fmt.Println(a) // 输出 [1, 2, 3]    
    fmt.Println(b) // 输出 [10, 2, 3]}
```
#### 9.数组遍历
在数组中:
**fori**
```go
func main() {  
    numbers := [...]int{1, 2, 3, 4, 5}  
    for i := 0; i < len(numbers); i++ {  
       fmt.Println(numbers[i])  
    }}
输出：
1
2
3
4
5
```
**forr**
```go
func main() {  
    numbers := [...]int{1, 2, 3, 4, 5}  
    for _, number := range numbers {  
       fmt.Println(number)  
    }}
输出：
1
2
3
4
5
```
# 0x09 切片
#### 1.定义切⽚
切⽚是⼀个**动态数组**，可以在运⾏时改变⻓度。切⽚的定义语法如下：
```
var sliceName []elementType
```
例如：
```go
var numbers []int
```
#### 2.初始化切⽚
字⾯量初始化：
```go
func main() {  
    numbers := []int{1, 2, 3, 4, 5}  
    fmt.Println(numbers)  
}
输出：[1 2 3 4 5]
```
make 函数创建切⽚：
```go
numbers := make([]int, 5) // 创建⼀个⻓度和容量都为 5 的切⽚

func main() {  
    ints := make([]int, 5)  
    ints[0] = 1  
    ints[1] = 2  
    ints[2] = 3  
    fmt.Println(ints)  
}
输出：[1 2 3 0 0]
```
指定容量：
```go
numbers := make([]int, 5, 10) // 创建⼀个⻓度为 5，容量为 10 的切⽚

func main() {  
    ints := make([]int, 2, 5)  
    ints[0] = 1  
    ints[1] = 2  
    fmt.Println(ints)  
}
输出：[1 2]
```
#### 3.切⽚的基本操作
访问和修改切⽚元素
切⽚元素通过索引访问，索引从 0 开始：
```go
func main() {  
    ints := []int{1, 2, 3, 4, 5}  
    fmt.Println(ints[0])  //输出第一个元素
    ints[1] = 10          //修改第二个元素的值
    fmt.Println(ints[1])  
}
输出：
1
10
```
获取切⽚⻓度和容量
```go
func main() {  
    ints := []int{1, 2, 3, 4, 5}  
    fmt.Println(len(ints))  //输出切片的长度
    fmt.Println(cap(ints))  //输出切片的容量
}
输出：
5
5
```
#### 4.切⽚的动态增⻓
切⽚可以使⽤ append 函数动态增⻓
```go
func main() {  
    ints := []int{1, 2, 3, 4, 5}  
    ints = append(ints, 6, 7, 8)  
    fmt.Println(ints)  
}
输出：[1 2 3 4 5 6 7 8]
```
#### 5.切⽚的遍历
可以使⽤ for 循环或 range 关键字遍历切⽚
**fori**
```go
func main() {  
    ints := []int{1, 2, 3, 4, 5}  
    for i := 0; i < len(ints); i++ {  
       fmt.Println(ints[i])  
    }}
输出：
1
2
3
4
5
```
**forr**
```
func main() {  
    ints := []int{7, 2, 3, 4, 5}  
    for _, i := range ints {  
       fmt.Println(i)  
    }}
输出：
7
2
3
4
5
```
#### 6.切⽚与数组的区别
**数组**
```
固定⻓度：数组的⻓度在声明时就确定了，不能动态改变。
值类型：数组是值类型，赋值或传参时会进⾏值拷⻉。、
性能：由于数组的⻓度固定，性能上会⽐切⽚更好。
```
**切⽚**
```
动态⻓度：切⽚的⻓度可以动态改变，更加灵活。
引⽤类型：切⽚是引⽤类型，赋值或传参时不会进⾏值拷⻉，⽽是引⽤同⼀个底层数组。
内置函数⽀持：切⽚有很多内置函数⽀持，如 append、copy 等。
```
#### forr 与 fori 的区别
**forr**
```
range 提供了⼀种简洁的⽅式来迭代数组、切⽚、映射和通道。使⽤ range 迭代数组时，它会返回两个值：索引和该索引处的值。这使得你可以直接访问数组的元素，⽽不需要显式地使⽤索引来访问。
```
**fori**
```
传统的 for 循环不会⾃动解包数组的值，因此需要使⽤索引来访问数组的元素
```
# 0x10 指针
#### 1.指针基础知识
指针的定义：
```go
跟&， &是取⼀个变量的内存地址，* 是指向这个地址的值，也就是解引⽤
& 符号：取地址，⽤于获取变量的内存地址。
** 符号：解引⽤，⽤于访问指针指向的变量的值。
指针的声明和初始化：
var ptr *int // 声明⼀个指向int类型的指针
var x int = 10
ptr = &x // ptr现在指向x的地址
```
指针的解引⽤
使⽤ * 操作符来访问指针指向的变量的值。
```go
fmt.Println(*ptr) // 输出10
```
详解：
```go
*是解引用，&是取地址

*
0xc00000e028 -->> 10

&
0xc00000e030 <<-- 11
```
举例：
`&`->取地址
```go
func main() {  
    a := 10  
    fmt.Println(&a)  
}
输出：0x140a0d8
```
`*`->空指针
```go
func main() {  
    var b *int  
    fmt.Println(b)  
}
输出：<nil>
```
然后取a的地址赋值给b，解引用b得到b的值
```go
func main() {  
    a := 10  
    var b *int  
    b = &a  
    fmt.Println(*b)  
}
输出：10
```
指针的使⽤场景
```
**函数参数传递**
使⽤指针可以避免在函数调⽤时复制⼤块数据，提⾼效率
```
示例：
```go
package main  
  
import "fmt"  
  
func Updata(a *int) {  //定义一个Updata方法,传入一个int型指针
    *a = 100  
}  
func b() {  //这里定义一个b方法进行测试使用。
    var a int = 10  
    fmt.Println("a=", a)  //输出10
    Updata(&a)  
    fmt.Println("a=", a)  //使用Updata方法，将a的地址传进去，然后Updata将a的地址进行修改。就变成了100
}  
  
func main() {  
    b() 
}
```
#### 2.结构体指针
使⽤指针可以直接修改结构体的字段
示例：
```go
package main  
import "fmt"  
type Test struct {  //定义一个结构化指针
    i int  
    s string  
}  
func main() {  
    t := &Test{  
       i: 1,  
       s: "2",  
    }    
    fmt.Println(*t)  
}
输出：{1 2}
```
#### 3.指针的注意事项
**空指针**
指针在未初始化时默认为 nil，使⽤前需要检查。
```
var ptr *int
if ptr != nil {
fmt.Println(*ptr)
}
```
**指针运算**
Go 语⾔不⽀持指针运算（如 C 语⾔中的指针加减运算）
# 0x11 类型转换
#### 1.基本概念
```
类型转换是将⼀种数据类型的值转换为另⼀种数据类型的值。
Go 语⾔是强类型语⾔，不同类型之间不能直接进⾏赋值或运算，必须通过显式的类型转换。
```
#### 2.语法
T 是⽬标类型。
v 是要转换的值。
```
T(v)
```
### 3.常⻅的类型转换
#### 浮点数类型之间的转换
```go
func main() {  
    i := 3.14  
    fmt.Println(int(i))  
}
输出；3
```
**注：浮点数转换为整数时，⼩数部分会被舍弃**
#### 整数类型之间的转换
```
var i int = 42
var f float64 = float64(i)
var u uint = uint(i)
```
#### 字符串和字节切⽚之间的转换
示例：
```go
func main() {  
    s := "hello"   //字符串  
    b := []byte(s) //转换成字符切片  
    fmt.Println(b) //[104 101 108 108 111]  
    b[0] = 'H'     //将b[0]号位,转换成大写的H  
    fmt.Println(b) //[72 101 108 108 111]  
    fmt.Println(string(b)) //然后在转过来，输出：Hello
}
```
# 0x12 Golang 包
**导⼊**
在 Go 语⾔中，导⼊包使⽤ import 关键字。可以导⼊标准库包，也可以导⼊⾃定义包。
```
import "fmt"
import "math"
```
**导⼊多个包**
```go
import (
	"fmt"
		"math"
)
```
**别名导⼊**
可以为导⼊的包指定⼀个别名，这样在使⽤包中的内容时可以使⽤别名：
```go
package main  
  
import (  
    f "fmt"  //起个别名：f
    m "math" 
)  
  
func main() {  
    f.Println("Hello, World!")  
    result := m.Sqrt(16)  
    f.Println(result)  
}
```
**导出名称**
```
在Go语⾔中，包中的标识符（变量、常量、类型、函数等）如果⾸字⺟⼤写，则表示该标识符是导出的，可以被其他包访问；如果⾸字⺟⼩写，则表示该标识符是未导出的，只能在包内访问。
```
空⽩标识符导⼊
有时需要导⼊⼀个包，但不直接使⽤包中的任何标识符。这种情况下可以使⽤空⽩标识符 _ ：
```go
import (
	_ "fmt"
)
```
# 0x13 字符串
### 1.字符串基础
字符串类型：
```
在 Go 中，字符串是不可变的字节序列。
字符串类型是 string。
```
字符串字⾯量：
```
双引号："Hello, World!"
反引号：`Hello, World!`，⽤于多⾏字符串和包含特殊字符的字符串。
```
### 2.字符串函数
#### Contains
检查字符串是否包含⼦字符串。
```go
package main  
  
import (  
    "fmt"  
    "strings" 
    )  
func main() {  
    fmt.Println(strings.Contains("hello,word!", "hello")) 
    //主要是对 hello,word!是否含有hello，如果有则是true，如果没有则是false，也区分大小写，完全匹配才行。
	}
输出：true
```
一般写POC，举例：
```go
package main  
import (  
    "fmt"  
    "github.com/imroc/req/v3"    
    "strings"
) 
  
func main() {
    client := req.C()  
    reps, err := client.R().Get("http://www.baidu.com")
    if err != nil {  
       panic(err)  
    }
    if strings.Contains(reps.String(), "virus-2020-clicked") {  
    //这句话的意思是相应包里是否含有virus-2020-clicked字符串，如果含有则返回Success如果不含有则返回Failed
       fmt.Println("Success")  
    } else {       
	    fmt.Println("Failed")  
    }
}
输出：Success
```
#### ContainsAny
检查字符串是否包含任何⼀个给定字符。
```go
func main() {  
    if strings.ContainsAny("hello word", "weo") {  
       //检查某个字符，hello word是否含有weo字符，如果含有则返回Success  
       fmt.Println("Success")  
    } else {       
	    fmt.Println("Failed")  
    }
}
输出：Success
```
#### ContainsRune
检查字符串是否包含指定的 rune 字符。
```go
func main() {  
    if strings.ContainsRune("hello word", 'h') {  
    //检查某个字符，hello word是否含有w字符，如果含有则返回Success  
    fmt.Println("Success")  
    } else {       
    fmt.Println("Failed")  
    }
}
输出：Success
```
#### Count
计算⼦字符串在字符串中出现的次数。
```go
func main() {  
    fmt.Println(strings.Count("hellohhhhhhhhh", "h"))  
}
输出：10
```
#### EqualFold
忽略⼤⼩写⽐较两个字符串是否相等。
```go
func main() {  
    fmt.Println(strings.EqualFold("Go", "go"))  
}
输出：true
```
#### Fields
将字符串按空⽩字符分割成切⽚。
```go
func main() {  
    fmt.Println(strings.Fields("hello world"))  
}
输出：[hello world]
```
#### HasPrefix
检查字符串是否以指定前缀开头。
```go
func main() {  
    fmt.Println(strings.HasPrefix("Hello word!!!", "hello"))  
}
输出：false 区分大小写。
```
#### HasSuffix
检查字符串是否以指定后缀结尾。
```go
func main() {  
    fmt.Println(strings.HasSuffix("Hello word", "word"))  
}
输出：true
```
#### Index
返回⼦字符串在字符串中第⼀次出现的位置。
```go
func main() {  
    fmt.Println(strings.Index("hello world", "world"))  
}
输出：6
```
#### 注意事项
1. **⼤⼩写敏感**：⼤多数函数都是⼤⼩写敏感的，如 Contains 、 Index 等。
2. **空字符串处理**：某些函数在处理空字符串时会返回特定结果，如 Split 会返回包含⼀个空字符串的切⽚。
```go
函数 函数作⽤
Contains：检查字符串是否包含⼦字符串
ContainsAny：检查字符串是否包含任何⼀个给定字符
ContainsRune：检查字符串是否包含指定的 rune 字符
Count：计算⼦字符串在字符串中出现的次数
EqualFold：忽略⼤⼩写⽐较两个字符串是否相等
Fields：将字符串按空⽩字符分割成切⽚
HasPrefix：检查字符串是否以指定前缀开头
HasSuffix：检查字符串是否以指定后缀结尾
Index：返回⼦字符串在字符串中第⼀次出现的位置
IndexAny：返回字符串中任意⼀个字符第⼀次出现的位置
IndexByte：返回指定字节在字符串中第⼀次出现的位置
IndexRune：返回指定 rune 字符在字符串中第⼀次出现的位置
Join：将字符串切⽚⽤指定分隔符连接成⼀个字符串
LastIndex：返回⼦字符串在字符串中最后⼀次出现的位置
LastIndexAny：返回字符串中任意⼀个字符最后⼀次出现的位置
Repeat：重复字符串指定次数
Replace：替换字符串中的⼦字符串
ReplaceAll：替换字符串中的所有⼦字符串
Split：将字符串按指定分隔符分割成切⽚
SplitAfter：将字符串按指定分隔符分割成切⽚，保留分隔符
SplitAfterN：将字符串按指定分隔符分割成切⽚，保留分隔符，最多分割 N 次
SplitN：将字符串按指定分隔符分割成切⽚，最多分割 N 次
Title：将字符串的每个单词的⾸字⺟⼤写
ToLower：将字符串转换为⼩写
ToUpper：将字符串转换为⼤写
Trim：去除字符串两端的指定字符
TrimSpace：去除字符串两端的空⽩字符
TrimPrefix：去除字符串前缀
TrimSuffix：去除字符串后缀
NewReplacer：创建⼀个字符串替换器
```
# 0x14 字符串格式化
## 1.Fmt
fmt.Print
### 常⻅占位符
%v：值的默认格式表示
```go
func main() {  
    fmt.Printf("%%v: %v\n", 123)  
}
输出：%v: 123
```
%+v：类似%v，但会添加字段名（结构体）
```go
func main() {  
    fmt.Printf("%%+v: %+v\n", struct{ Name string }{"Alice"})  
}
输出：%+v: {Name:Alice}
```
%#v：值的Go语法表示
```go
func main() {  
    fmt.Printf("%%#v: %#v\n", struct{ Name string }{"Alice"})  
}
输出：%#v: struct { Name string }{Name:"Alice"}
```
%T：值的类型
```go
func main() {  
    fmt.Printf("%%T: %T\n", 123)  
}
输出：%T: int
```
% %：百分号本身
```go
func main() {  
    fmt.Printf("%%%%: %%\n")  
}
输出：%%: %
```
### 布尔值
%t：单词true或false
```go
func main() {  
    fmt.Printf("%%t: %t\n", true)  
}
输出：true
```
### 整数
%b：⼆进制表示
```go
func main() {  
    fmt.Printf("%%b: %b\n", 123)  
}
输出：%b: 1111011
```
%c：相应Unicode码点表示的字符
```go
func main() {  
    fmt.Printf("%%c: %c\n", 65)  
}
输出：%c: A
```
%d：⼗进制表示
```go
func main() {  
    fmt.Printf("%%d: %d\n", 123)  
}
输出：%d: 123
```
%o：⼋进制表示
```go
func main() {  
    fmt.Printf("%%o: %o\n", 123)  
}
输出：%o: 173
```
%O：带0o前缀的⼋进制表示
```go
func main() {  
    fmt.Printf("%%O: %O\n", 123)  
}
输出：%O: 0o173
```
%q：单引号围绕的字符字⾯值，必要时会采⽤转义表示
```go
func main() {  
    fmt.Printf("%%q: %q\n", 65)  
}
输出：%q: 'A'
```
%x：⼗六进制表示，使⽤a-f
```go
func main() {  
    fmt.Printf("%%x: %x\n", 123)  
}
输出：%x: 7b
```
%X：⼗六进制表示，使⽤A-F
```go
func main() {  
    fmt.Printf("%%X: %X\n", 123)  
}
输出：%X: 7B
```
%U：Unicode格式：U+1234，等同于"U+%04X"
```go
func main() {  
    fmt.Printf("%%U: %U\n", 123)  
}
输出：%U: U+007B
```
### 浮点数和复数
%b：⽆⼩数部分的指数为⼆的幂的科学计数法，如-123456p-78
```go
func main() {  
    fmt.Printf("%%b: %b\n", 123.456)  
}
输出：%b: 8687443681197687p-46
```
%e：科学计数法，如-1234.456e+78
```go
func main() {  
    fmt.Printf("%%e: %e\n", 123.456)  
}
输出：%e: 1.234560e+02
```
%E：科学计数法，如-1234.456E+78
```go
func main() {  
    fmt.Printf("%%E: %E\n", 123.456)  
}
输出：%E: 1.234560E+02
```
%f：有⼩数点⽽⽆指数，如123.456
```go
func main() {  
    fmt.Printf("%%f: %f\n", 123.456)  
}
输出：%f: 123.456000
```
%F：等同于%f
```go
func main() {  
    fmt.Printf("%%F: %F\n", 123.456)  
}
输出：%F: 123.456000
```
%g：根据实际情况采⽤%e或%f格式（以获得更紧凑、准确的输出）
```go
func main() {  
    fmt.Printf("%%g: %g\n", 123.456)  
}
输出：%g: 123.456
```
%G：根据实际情况采⽤%E或%F格式（以获得更紧凑、准确的输出）
```go
func main() {  
    fmt.Printf("%%G: %G\n", 123.456)  
}
输出：%G: 123.456
```
### 字符串和字节切⽚
%s：解析变量，直接打印值
```go
func main() {  
    name := "Go"  
    fmt.Printf("Hello, %s!\n", name)  
}
输出：Hello, Go!
```
%q：打印值的时候加上双引号
```go
func main() {  
    fmt.Printf("%%q: %q\n", "hello")  
}
输出：%q: "hello"
```
%x：每个字节⽤两字符⼗六进制数表示（使⽤a-f）
```go
func main() {  
    fmt.Printf("%%x: %x\n", "hello")  
}
输出：%x: 68656c6c6f
```
%X：每个字节⽤两字符⼗六进制数表示（使⽤A-F）
```go
func main() {  
    fmt.Printf("%%X: %X\n", "hello")  
}
输出：%X: 68656C6C6F
```
### 指针
%p：⼗六进制表示，前缀0x
```go
func main() {  
    fmt.Printf("%%p: %p\n", &struct{}{})  
}
输出：%p: 0xacdd40
```
**注意事项：**
```
注意⼤⼩写区分
```
## 2.fmt.Scanf
```go
func main() {  
    var (       
    name  string  
    age   int  
    score float64  
    )  
    fmt.Println("请输⼊你的名字、年龄和分数（例如：张三 25 89.5）：")  
    _, err := fmt.Scanf("%s %d %f", &name, &age, &score)  
    if err != nil {  
	       fmt.Println("输⼊错误：", err)  
	       return    
       }    
    fmt.Printf("名字：%s\n", name)    // %s 表示字符串  
    fmt.Printf("年龄：%d\n", age)     // %d 表示整数  
    fmt.Printf("分数：%.2f\n", score) // %.2f 表示保留两位⼩数的浮点数  
}
输出：
请输⼊你的名字、年龄和分数（例如：张三 25 89.5）：
李勐 23 23
名字：李勐
年龄：23
分数：23.00
```
**注意事项：**
```
Scanf如果是单次获取多个值，使⽤空格隔开，并⾮回⻋
fmt.Scanf 的格式化字符串中，逗号（,）会被视为输⼊的⼀部分，因此⽤户在输⼊时需要包含逗号。使⽤空格作为分隔符。
```
# 0x15 条件控制
## 1.if
语法：
```
if condition {
// 执⾏的代码
}
```
示例：
```go
package main  
  
import "fmt"  
  
func main() {  
    x := 10  
    // 基本的if语句  
    if x > 5 {  
       fmt.Println("x⼤于5")  
    }    // if-else语句  
    if x > 15 {  
       fmt.Println("x⼤于15")  
    } else {       
	    fmt.Println("x不⼤于15")  
    }    // if-else if-else语句  
    if x < 5 {  
       fmt.Println("x⼩于5")  
    } else if x == 10 {  
       fmt.Println("x等于10")  
    } else {       
	    fmt.Println("x⼤于5且不等于10")  
    }    // 带初始化语句的if  
    if y := 20; y > x {  
       fmt.Println("y⼤于x")  
    } else {       
    fmt.Println("y不⼤于x")  
    }
}

输出：
x⼤于5
x不⼤于15
x等于10
y⼤于x
```
注意事项：
```
基本的if语句：⽤于判断条件是否为真，如果为真则执⾏代码块内容。
if-else语句：在条件为假时执⾏else代码块。
if-else if-else语句：⽤于多个条件的判断。
带初始化语句的if：可以在if语句中初始化⼀个变量，但该变量的作⽤域仅限于if语句块内。

Go语⾔中的if语句不需要括号包围条件表达式。
if语句中的初始化语句和条件表达式之间⽤分号分隔。
if语句块中的变量作⽤域仅限于该语句块内。
```
## 2.Switch
基本示例
```go
package main  
  
import "fmt"  
  
func main() {  
    day := "Tuesday"  
    switch day {  
    case "Monday":  
       fmt.Println("今天是星期⼀")  
    case "Tuesday":  
       fmt.Println("今天是星期⼆")  
    case "Wednesday":  
       fmt.Println("今天是星期三")  
    default:  
       fmt.Println("其他⽇⼦")  
    }}
输出：今天是星期⼆
```
多个条件
```go
package main  
  
import "fmt"  
  
func main() {  
    day := "Saturday"  
    switch day {  
    case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":  
       fmt.Println("⼯作⽇")  
    case "Saturday", "Sunday":  
       fmt.Println("周末")  
    default:  
       fmt.Println("⽆效的⽇⼦")  
    }
}
输出：周末
```
## 3.For
Go语⾔中的for循环有三种基本形式：
```
经典形式：类似于C语⾔的for循环。
条件形式：只有⼀个条件表达式。
⽆限循环：没有条件表达式。
```
经典形式
```
for 初始化语句; 条件表达式; 后置语句 {
// 循环体
}
```
示例代码
```go
package main  
  
import "fmt"  
  
func main() {  
    for i := 0; i < 5; i++ {  
       fmt.Println(i)  
    }  
}
输出：
0
1
2
3
4
```
条件形式
```go
package main  
import "fmt"  
func main() {  
    i := 0  
    for i < 5 {  
       fmt.Println(i)  
       i++  
    }}
输出：
0
1
2
3
4
```
## 4.range关键字
### 遍历数组、切⽚
```go
package main  
import "fmt"  
func main() {  
    arr := []int{1, 2, 3, 4, 5}  
    for index, value := range arr {  
       fmt.Printf("Index: %d, Value: %d\n", index, value)  
    }
}
输出：
Index: 0, Value: 1
Index: 1, Value: 2
Index: 2, Value: 3
Index: 3, Value: 4
Index: 4, Value: 5
```
### 遍历映射
```go
package main  
  
import "fmt"  
  
func main() {  
    m := map[string]int{"a": 1, "b": 2, "c": 3}  
    for key, value := range m {  
       fmt.Printf("Key: %s, Value: %d\n", key, value)  
    }
}
输出：
Key: a, Value: 1
Key: b, Value: 2
Key: c, Value: 3
```
### 遍历字符串
```go
package main  
  
import "fmt"  
  
func main() {  
    str := "hello"  
    for index, char := range str {  
       fmt.Printf("Index: %d, Char: %c\n", index, char)  
    }}
输出：
Index: 0, Char: h
Index: 1, Char: e
Index: 2, Char: l
Index: 3, Char: l
Index: 4, Char: o
```
### 遍历通道
```go
package main  
  
import "fmt"  
  
func main() {  
    ch := make(chan int, 5)  
    ch <- 1  
    ch <- 2  
    ch <- 3  
    close(ch)  
  
    for value := range ch {  
       fmt.Println(value)  
    }
}
输出：
1
2
3
```
## 5.break和continue
```
break：⽤于退出循环。

continue：⽤于跳过当前循环的剩余部分，直接进⼊下⼀次循环。
```
### break
```go
package main  
  
import "fmt"  
  
func main() {  
    for i := 0; i < 10; i++ {  
       if i == 5 {  
          break       
          }       
    fmt.Println(i)  
    }
}
输出：
0
1
2
3
4
```
### continue
```go
package main  
  
import "fmt"  
  
func main() {  
    for i := 0; i < 10; i++ {  
       if i == 5 {  
          continue       
          }       
    fmt.Println(i)  
    }
}
输出：
0
1
2
3
4
6
7
8
9
```
# 0x16 结构体&Maps
## 1.定义结构体
结构体是由⼀系列具有相同类型或不同类型的数据构成的数据集合。结构体是Go语⾔中⼀种重要的数据类型。
```go
type Person struct {  
    Name string  
    Age int  
}
```
## 2.结构体字⾯量
结构体字⾯量⽤于创建结构体实例。
```
p1 := Person{Name: "Alice", Age: 30}
p2 := Person{"Bob", 25} // 省略字段名
```
示例：
```go
package main  
  
import "fmt"  
  
type Person struct {  
    Name string  
    Age  int  
}  
  
func main() {  
    person1 := Person{Name: "John", Age: 25}  
    person2 := Person{Name: "Jane", Age: 35}  
    fmt.Println(person1)  
    fmt.Println(person2)  
}
输出：
{John 25}
{Jane 35}
```
## 3.访问和修改结构体字段
可以通过点操作符访问和修改结构体字段。
```go
package main  
  
import "fmt"  
  
type Person struct {  
    Name string  
    Age  int  
}  
  
func main() {  
    p := Person{Name: "Alice", Age: 30}  
    fmt.Println(p.Name) // 输出: Alice  
    p.Age = 31  
    fmt.Println(p.Age) // 输出: 31  
  
}
```
## 4.结构体嵌套
结构体可以嵌套，即⼀个结构体可以包含另⼀个结构体作为字段。
```go
package main  
  
import "fmt"  
  
type Address struct {  
    City, State string  
}  
type Person struct {  
    Name    string  
    Age     int  
    Address Address  
}  
  
func main() {  
    p := Person{  
       Name: "Alice",  
       Age:  30,  
       Address: Address{  
          City:  "New York",  
          State: "NY",  
       },    
    }    
       fmt.Println(p.Address.City) // 输出: New York  
}
```
## 5.结构体与maps
可以使⽤结构体作为map的值。
```go
package main  
import "fmt"  
type Person struct {  
    Name string  
    Age  int  
}  
func main() {  
    people := map[string]Person{  
       "Alice": {Name: "Alice", Age: 30},  
       "Bob":   {Name: "Bob", Age: 25},  
    }    
    fmt.Println(people["Alice"].Age) // 输出: 30  
}
```
## 6.指向结构体的指针
可以创建指向结构体的指针，并通过指针访问和修改结构体字段。
```go
package main  
import "fmt"  
type Person struct {  
    Name string  
    Age  int  
}  
func main() {  
    p := Person{Name: "Alice", Age: 30}  
    pPtr := &p  
    fmt.Println(pPtr.Name) // 输出: Alice  
    pPtr.Age = 31  
    fmt.Println(p.Age) // 输出: 31  
}
```
## 7.⽅法与结构体
可以为结构体定义⽅法
```go
package main  
  
import "fmt"  
  
type User struct {  
    Name string  
    Age  int  
}  
  
func (U User) GetName() {  
    fmt.Println(U.Name, U.Age)  
}  
func main() {  
    user := User{}  
    user.Name = "John"  
    user.Age = 25  
    user.GetName()  //输出：John 25
}
```
## 8.结构体⽅法中的指针接收者
使⽤指针接收者可以修改结构体的字段。
```go
package main  
  
import "fmt"  
  
type User struct {  
    Name string  
    Age  int  
}  
  
func (U *User) GetName() {  
    U.Age++  
}  
func main() {  
    u := User{"John", 23}  
    fmt.Println(u)  //输出：{John 23}
    u.GetName()     //输出：{John 24}
    fmt.Println(u)  
}
```
## 9.匿名结构体
匿名结构体是⼀种不需要事先定义类型的结构体。
```go
package main  
  
import "fmt"  
  
func main() {  
    p := struct {  
       Name string  
       Age  int  
    }{  
       Name: "Alice",  
       Age:  30,  
    }    
    fmt.Println(p.Name) // 输出: Alice  
}
```
# 0x17 函数
多参数
```go
package main  
  
import "fmt"  
  
func add(a int, b int) int {  
    return a + b  
}  
func main() {  
    result := add(3, 4)  
    fmt.Println(result) // 输出: 7  
}
```
多返回值
```go
package main  
  
import "fmt"  
  
func divide(a int, b int) (int, int) {  
    quotient := a / b  
    remainder := a % b  
    return quotient, remainder  
}  
func main() {  
    q, r := divide(10, 3)  
    fmt.Println(q, r) // 输出: 3 1  
}
```
匿名函数
```go
package main  
  
import "fmt"  
  
func main() {  
    func(msg string) {  
       fmt.Println(msg)  
    }("Hello, World!") // 输出: Hello, World!  
}
```
初始化函数
```go
package main  
  
import "fmt"  
  
func init() {  
    fmt.Println("初始化函数")  
}  
func main() {  
    fmt.Println("主函数")  
}
```
可变参数函数
```go
package main  
  
import "fmt"  
  
func sum(nums ...int) int {  
    total := 0  
    for _, num := range nums {  
       total += num  
    }  
    return total  
}  
func main() {  
    result := sum(1, 2, 3, 4)  
    fmt.Println(result) // 输出: 10  
}
```
命名返回值
```go
package main  
  
import "fmt"  
  
func swap(a, b int) (x int, y int) {  
    x = b  
    y = a  
    return  
}  
func main() {  
    a, b := swap(1, 2)  
    fmt.Println(a, b) // 输出: 2 1  
}
```
闭包
闭包可以理解成定义在⼀个函数内部的函数，在本质上，闭包是将函数和函数外部连接在⼀起的，或者说是函数和其引⽤环境的组合体。
闭包指的是⼀个函数和与其相关的引⽤环境组合⽽成的实体，简单来说就是 
**闭包=函数+引⽤环境**
```go
package main  
  
import "fmt"  
  
func adder() func(int) int {  
    sum := 0  
    return func(x int) int {  
       sum += x  
       return sum  
    }  
}  
func main() {  
    pos, neg := adder(), adder()  
    for i := 0; i < 10; i++ {  
       fmt.Println(pos(i), neg(-2*i))  
    }}
输出：
0 0
1 -2
3 -6
6 -12
10 -20
15 -30
21 -42
28 -56
36 -72
45 -90
```
