---
title: C-存储类与运算符
description: 先存再算
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-01 16:58 +0800
nav:
  prev:
    path: posts/c/c-variable-and-const
    title: C-变量与常量
  next:
    path: posts/c/c-scope
    title: C-作用域
---

## 储存类

程序运行在内存上，变量与函数都储存在内存上，存储类定义 `C` 程序中变量/函数的存储位置、生命周期和作用域。储存类上是一系列修饰符，用于在变量或函数声明时。储存类有以下四种：

- extern

- register

- static

- auto

### extern

 `extern` 存储类用于定义在其他文件中声明的全局变量或函数。 `extern` 关键字用来指示编译器不用为声明的变量分配储存空间，而是会在其他文件内定义。

 `extern` 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的。使用 `extern` 时，对于无法初始化的变量，会把变量名指向一个之前定义过的存储位置。

> extern的意义在于引用另一个源文件内的全局变量或函数。extern存储类告知编译器，这个玩意是存在的，你先用就行。编译器在链接阶段，会查找符号表，找到extern对应的变量或函数的真实引用。

```c
//1.c

#include <stdio.h>

int extern_count;
extern void write_extern();
 
int main()
{
   extern_count = 5;
   write_extern();
}

//2.c

#include <stdio.h>

extern int extern_count;//extern_count已在1.c中定义
 
void write_extern()
{
   printf("count is %d\n", extern_count);
}
```

编译时，需要两个源文件一起编译或者分别编译并手动链接另一个二进制文件。

```bash
#bash

gcc 1.c 2.c
```

### register

 `register` 意为寄存器，是CPU在运算时临时存放数据的位置，位于CPU内部。 `register` 存储类定义变量存储在寄存器上，变量的最大尺寸取决于寄存器大小，一般为1字节，不能取地址（因为根本不在内存上），但是因为在CPU内部，访问速度极快。

> CPU在进行运算时会先将数据从内存搬运到寄存器再进行计算，存储在寄存器上的变量可以省去这一步骤，需要频繁访问的变量上使用 register 存储类可以提高程序的运行速度。

此外，如果变量被定义为 `register` 则不一定会被真的放在寄存器上，这取决于变量本身和硬件条件（编译器和操作系统肯定不会把一个8字节大小的double变量往寄存器上放，寄存器就一个字节）。

```c
register int counter;
```

### static

 `static` 存储类声明静态变量，存储于程序的静态内存区，不会随着栈的回收而回收，在程序的完整生命周期内只会进行一次初始化，即第一次初始化，后续声明+初始化会被忽略。

> 由 `static` 声明的全局变量的可见性会被局限在当前文件，可以用来缩窄变量的可访问性。

```c
#include <stdio.h>

void operation();

static int count = 10;//全局静态变量，本文件可见

int main()
{
    while (count--) {
        operation();
    }
    return 0;
}

void operation()
{            
  static int thingy = 5;//对静态变量的初始化只会进行一次，局部变量也一样
  thingy++;
  printf(" thingy = %d ， count = %d\n", thingy, count);
}
```

结果如下，tiny变量只初始化了一次。

```txt
 thingy = 6 ， count = 9
 thingy = 7 ， count = 8
 thingy = 8 ， count = 7
 thingy = 9 ， count = 6
 thingy = 10 ， count = 5
 thingy = 11 ， count = 4
 thingy = 12 ， count = 3
 thingy = 13 ， count = 2
 thingy = 14 ， count = 1
 thingy = 15 ， count = 0
```

### auto

 `auto` 存储类是所有局部变量默认的存储类。所有函数和变量在声明时都默认使用 `auto` 存储类。

## 运算符

在变量存储好后，就可以进行运算了。运算符是一种告诉编译器执行特定的数学或逻辑操作的符号， `C` 中的运算符分为以下几类：

- 算术运算符

- 关系运算符

- 逻辑运算符

- 位运算符

- 赋值运算符

- 杂项运算符

- 逗号运算符

- 访问运算符

运算符两侧或运算符作用的数称为操作数。

### 算术运算符

算术运算符，即基本的四则运算加上取模、自增、自减。

以下假设A=24,B=15

|运算符|描述|效果|
|:-:|:-:|:-:|
|+|把两个操作数相加|A + B = 39|
|-|第一个操作数减去第二个操作数|A - B = 9|
|*|把两个操作数相乘|A * B = 360|
|/|第一个操作数除以第二个操作数|A / B = 1（由于显示类型转换的特性，此处结果仍为整数）|
|%|第一个操作数整除第二个操作数的余数（取模）|A % B = 9|
|++|自增运算符，整数操作数值增加1|A++或++A得到25|
|--|自减运算符，整数操作数值减少1|B--或--B得到14|

所有算术运算符的结果均为右值。

> 自增减运算符连同操作数本身可以作为右值。其具体值取决于自增减运算符在操作数的位置。运算符在操作数的左侧表达式会先执行自增减操作再以操作后的操作数作为表达式的值；运算符在操作数的右侧表达式会先操作数作为表达式的值再执行自增减操作。

```c
int value = 100;
int a = ++value;//a = 101,value = 101
int b = value++;//b = 101,value = 102
int c = --value;//c = 101,vlaue = 101
int d = value--;//d = 101,vlaue = 100
```

### 关系运算符

关系运算符用于比较变量或表达式之间的关系

以下假设A=24,B=15

|运算符|描述|效果|
|:-:|:-:|:-:|
|==|检查两个操作数的值是否相等,相等则为真|A == B为假|
|!=|检查两个操作数的值是否不相等，不相等则为真|A != B为真|
|>|检查左操作数的值是否大于右操作数的值，大于则为真|A > B 为真|
|>=|检查左操作数的值是否大于等于右操作数的值，大于等于则为真|A >= B 为真|
|<|检查左操作数的值是否小于右操作数的值，小于则为真|A < B 为假|
|<=|检查左操作数的值是否小于等于右操作数的值，小于等于则为真|A <= B 为假|

> 在其他编程语言中会用一种名为 `Boolen` （即布尔）的类型表示逻辑真假。 `C` 没有布尔类型，取而代之的是任何为0的数字类型或者为空的指针会被视为逻辑假；其余所有值会被视为逻辑真。而表达式产生的逻辑真会被视为整数1,逻辑假会被视为整数0。

```c
int a = (1 == 1);//1 == 1为真，被视为1,a的值为1

int* b = NULL;
if (b)
{
    printf("b is not NULL\n");
}
else
{
    printf("b is NULL\n");
}
//输出b is NULL
```

所有关系运算符的结果均为右值。

### 逻辑运算符

用于逻辑关系式。

以下假设A=1,B=0

|运算符|描述|效果|
|:-:|:-:|:-:|
|&&|逻辑与，如果两个操作数均为真，结果为真|A && B为假|
|\|\||逻辑或，如果两个操作数至少有一个为真，结果为真|A \|\| B为真|
|!|逻辑非，结果为操作数的反转<br>操作数为真结果为假；操作数为假结果为真|!A 为假，!B为真|

> 逻辑与和逻辑或均可连续使用：<br> `<expr1> && <expr2> && <expr3> ...` 、<br> `<expr1> || <expr2> || <expr3> ...`

> **运算短路**<br>逻辑与和逻辑或均存在运算短路机制。当靠前的表达式的值已经足够确定整个逻辑表达式的值时，不会再执行后续表达式并直接给出结果。

```c
int a = 0;
int b = a && a++;//a为0,被判断为假，此时逻辑与语句值已经确定为假，后续的a++语句会被跳过
int c = 1;
int d = c || a += 114514;//c为1,被判断为真，此时逻辑与语句已经确定为真，后续语句被跳过
```

所有逻辑运算符的结果均为右值。

### 位运算符

变量储存在内存中，基本单位是字节，一个字节又包含8个位。为运算符即是对位进行操作。

若两个操作数占用的字节数不同则会在占用字节较少的变量的高位补0,使两个数等长。

以下假设A = 57（二进制0b00111001）,B=13（二进制0b00001101）

|运算符|描述|效果|
|:-:|:-:|:-:|
|&|按位与，每一位分别取逻辑与|A & B = 0b00001001|
|\||按位或，每一位分别取逻辑或|A \| B = 0b00111101|
|^|按位异或，每一位分别取逻辑异或|A ^ B = 0b00110100|
|~|按位取反，每一位分别取逻辑反|~A = 0b11000110|
|<<|向左移位，右空位补0，左侧溢出则忽略，即乘以<h>$2^n$</h>|A << 2 = 0b11100100|
|>>|向右移位，左空位补0，右侧溢出则忽略，即除以<h>$2^n$</h>|A >> 2 = 0b00001110|

所有位运算符的结果均为右值。

### 赋值运算符

赋值运算符用于为左值赋值。

以下假设A=24,B=15

|运算符|描述|效果|
|:-:|:-:|:-:|
|=|简单的赋值运算符，将右侧值赋值给左侧|A = B，结果A=B=15|
|+=|加且赋值运算符，把右边操作数加上左边操作数的结果赋值给左边操作数|A += B等效于A = A + B|
|-=|减且赋值运算符，把左边操作数减去右边操作数的结果赋值给左边操作数|A -= B等效于A = A - B|
|\*=|乘且赋值运算符，把右边操作数乘以左边操作数的结果赋值给左边操作数|A \*= B等效于A = A \* B|
|/=|除且赋值运算符，把左边操作数除以右边操作数的结果赋值给左边操作数|A /= B等效于A = A / B|
|%=|求模且赋值运算符，求两个操作数的模赋值给左边操作数|A %= B等效于A = A % B|
|<<=|左移且赋值运算符|A <<= B等效于A = A << B|
|>>=|右移且赋值运算符|A >>= B等效于A = A >> B|
|&=|按位与且赋值运算符|A &= B等效于A = A & B|
|\|=|按位异或且赋值运算符|A \|= B等效于A = A \| B|
|^=|按位或且赋值运算符|A ^= B等效于A = A ^ B|

> 赋值运算符同样有结果。其结果为左运算数的最终值。 `A += B` 结果为39

所有赋值运算符的结果均为右值。

### 杂项运算符

杂项运算符是 `C` 中未被归进以上几类的运算符。

|运算符|描述|效果|
|:-:|:-:|:-:|
|sizeof|返回变量或类型占据内存空间的大小|sizeof(int)结果为4|
|&|取址运算符，返回变量的内存地址（指针）|&a获得指向a变量的指针|
|*|返回内存地址（指针）对应的变量|*ptr访问指针所指的变量|
|? :|条件表达式|三目运算符|

> `? :` 用法为 `<condition> ? <expr1> : <expr2>` ， `condition` 为真时结果为 `expr1` ， `expr2` 不执行；为假时结果为 `expr2` ， `expr1` 不执行。

### 逗号运算符与括号表达式

还有一类特殊的表达式，括号表达式，可以用一个括号集合多个表达式，表达式之间以逗号分隔，其结果为最后一个表达式的结果。

```c
int a = (1,2,3,4);//a值为4（最后一个表达式）
```

### 访问运算符

这是一类类似于后缀的运算符，包括 `.` 、 `->` 和 `[]` ，用于访问变量或指针的对应功能或属性。这部分内容将在涉及到的地方解释。

## 运算符优先级

数学有四则运算，而运算之间存在优先级，指导我们分析复杂的算式。在 `C` 中有如此多运算符，同样有运算符优先级规则指导。

在下表中，运算符优先级从上往下、从左往右依次递减，同一行的运算符优先级均大于下一行。

|类别|运算符|结合性|
|:-:|:-:|:-:|
|后缀|() [] -> . ++ --|从左到右|
|一元|+ - ! ~ ++ -- (type)* & sizeof|从右到左|
|乘除|* / % |从左到右|
|加减|+ -|从左到右|
|移位|<< >>|从左到右|
|关系|< <= > >=|从左到右|
|相等|== !=|从左到右|
|位与|&|从左到右|
|位异或|^|从左到右|
|位或|\||从左到右|
|逻辑与|&&|从左到右|
|逻辑或|\|\||从左到右|
|条件|?:|从右到左|
|赋值|= += -= *= /= %=>>= <<= &= ^= \|=|从右到左|
|逗号|,|从左到右|

> 结合性指多个相同运算符连续使用时的计算顺序，从左到右即从左往右依次计算；从右到左则是从右往左依次计算。<br> `1+2+3` 会先计算 `1+2` 再计算 `3+3`

## 写在最后

运算符与变量共同组合成程序的基本骨架，程序中绝大部分操作都是由运算符完成。
