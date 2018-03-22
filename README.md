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
