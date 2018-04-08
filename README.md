## Go语言学习笔记
#### 1.简单的输出示例
>go run 文件名:直接执行文件
>编译:go build 文件名.会生成对应的可执行文件
```
package main

import "fmt"

func main() {
	fmt.Println("Hello,world!")
}

package别名: import io "fmt"

```
#### 2.import的包必须要用到,否则报错
>格式化代码 gofmt -w 文件名
```
import格式:
import (
    "fmt"
    "os"
)

Fprint format:
%d 十进制整数
%x, %o, %b 十六进制，八进制，二进制整数。
%f, %g, %e 浮点数： 3.141593 3.141592653589793 3.141593e+00
%t 布尔：true或false
%c 字符（rune） (Unicode码点)
%s 字符串
%q 带双引号的字符串"abc"或带单引号的字符'c'
%v 变量的自然形式（natural format）
%T 变量的类型
%% 字面上的百分号标志（无操作数）

```
#### 3.声明变量
```
Go语言主要有四种类型的声明
语句：var、const、type和func，分别对应变量、常量、类型和函数实体对象的声明
:=简短声明,其中必须要包含一个新的变量
小写字母开头的变量和方法属于private,只能包内调用
大写字符开头的变量和方法属于public,包外也可以调用
全局变量要用 var a int = 123这样来声明
int转换为string类型
a++只能作为表达式无法作为赋值

```
#### 4.常量
```
常量组中如果只定义了而不声明值,那么值就于此定义的上一行相同,每一行数量相同,常量大写.
iota:在常量组中使用,每多一个常量iota的值会+1,默认是0.新定义一个重新置零.
只想使用private的常量命名:_MAXLength=1

```
#### 5.运算符
```
^可作为一元和二元运算符:
一元优先级高于二元运算符
一元:fmt.Printf("%d",^1)
二元:fmt.Printf("%d",2^1)
<< >>:左移右移
/*
  6:0110
  11:1011
  --------------
  &:与,同为1结果为1.0010
  |:或,有一个是1结果为1.1111
  ^:只有一个是1结果才为1.1101
  &^:如果第二个位上位1,则第一个数相同的位置上则置为0.0100
  &&两个同时为true
*/

```
#### 6.控制语句
>if语句也可以赋值,赋值会覆盖上面的初始化值
```
循环只有for语句:
1.无限循环
for {
    xxxxx
    break;   
}
2.带一个表达式
for a<=3{
    xxxx
}
3.经典形式
条件表达式最好是一个变量,加快速度
for i:=0;i<3;i++{
    xxxx
}
switch语句:符合一个case之后会自动调用break;如果需要继续执行的话就需要调用fallthrough语句
所有语句块中初始化的变量都是一个局部变量

```
#### 7.跳转语句
>goto,break,contiune
```
标签:LABEL
break可以跳出一个标记的LABEL
continue
goto会跳转到LABEL

```
#### 8.数组
```
var varname [n]<type>
a:=[2]int{1,1}
不指定长度: a:=[...]int{1,2,3,4}
数组无法直接比较大小,需要长度一样的数组才能进行比较,比较相等:== !=,两个数组相等要里面的元素排列一样
数组是值类型
//冒泡
a := [...]int{1, 3, 643, 675, 423, 213, 3213, 4, 324, 213, 213, 213, 213, 214, 324, 2}
b := len(a)
for i := 0; i < b; i++ {
    for j := i + 1; j < b; j++ {
        if a[i] > a[j] {
            c := a[i]
            a[i] = a[j]
            a[j] = c

        }
   }
}

```
#### 9.切片
>不定长数组
```
引用类型，值改变所有的slice的值都改变了
len长度,cap分配容量
[10]a;b:=a[2:4] len(b)=2 cap(b)=7
切片是引用类型,如果引用到了相同index的数值发生了变化另一个切片对应的index的值也会变化
copy(s1,s2)将s2的值复制到S1
s1 :=[]int{4,5,6}
s2 :=[]int{1,2,3,45,2}
copy(s2,s1)
fmt.Println(s2)
S2：[4 5 6 45 2]
for _,v:=range m {
    v=make(map[int]string)
    v[1] = "aaa"
}
类似上面的复制是不成功的,因为v只是一个拷贝不是一个引用,附了值不会对原来的slice产生影响

```
#### 10.map
```
相当于python的dict
make(map[int]string)
迭代:
for k,v:=range a

```
#### 11.function
```
func A(a int,b string)string
不定长参数
func B(a ...int)对应a是一个slice,参数为最后一个,值的拷贝
函数也可以作为类型
匿名函数:
a:= func(){
    fmt.Println("checknow")
}
a()
闭包:
func main() {
    f:=close(1)
    fmt.Println(f(2))

}
func close(x int)func(int)int{
    return func(i int) int {
        return i+x
    }
}
defer:
调用顺序的相反顺序来执行
发生错误也会执行
支持匿名函数调用
常用于文件关闭，资源清理
panic,recover
panic可以在任何地方触发，recover只有在使用了defer的语句才能使用
panic抛出异常
defer要在panic之前才会生效
defer func() {
if err:=recover();err!=nil{
    fmt.Println("recover in B")
    }
}()
panic("This is a panic")

示例:
func main() {
    var fs = [4]func(){}
    for i := 0; i < 4; i++ {
    //此处的i是值的拷贝
    defer fmt.Println("defer i=", i)
    //此处的i指向的是i的地址,下面循环调用到的时候I地址的值已经变为4
    defer func() { fmt.Println("defer_closure i=", i) }()
    fmt.Println(i)
    fs[i] = func() {
    //此处的i指向的是i的地址,下面循环调用到的时候I地址的值已经变为4
    fmt.Println("closure i=", i)
        }
}

for _,i:=range fs{
    i()
    }
}
输出:
closure i= 4
closure i= 4
closure i= 4
closure i= 4
defer_closure i= 4
defer i= 3
defer_closure i= 4
defer i= 2
defer_closure i= 4
defer i= 1
defer_closure i= 4
defer i= 0

```
