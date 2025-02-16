---
title: C-变量与常量
description: 可变的...常量？
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-27 16:58 +0800
nav:
  prev:
    path: posts/c/c-firstclass
    title: C-入门第一课
  next:
    path: posts/c/c-storage-and-operator
    title: C-存储类与运算符
--- 

## 变量

`C`的所有变量都需要先声明后使用。变量其实只不过是程序可操作的内存区域的名称，每个变量都有特定的类型，称为变量的数据类型。数据类型决定了变量存储的大小和布局。变量的类型在被声明时就固定死了，在程序运行期间不可改变。

`C`的变量声明遵从此格式：`<type-name> identifier [= value]`，`type-name`为变量类型，`identifier`为变量名。在声明变量时，可以同时初始化变量。若变量未初始化，数组变量的每个元素或函数内声明的非静态变量为随机值，指针为NULL，全局变量和静态变量变量的值均为0。可以使用`extern`关键字，让变量在其他文件内被定义，在当前位置只声明。

> 通常情况下，从堆上申请的变量也遵循此规则，但是在`Visual Studio`的Debug模式下，申请到的内存会被初始化为特定值（这种处理会产生令人津津乐道的“屯屯屯”和“烫烫烫”）。

> 此外，在`Visual Studio`中（非命令行cl），未初始化的数组内的元素会被初始化为0（堆栈均同）。

```c
//C

char var[64]; //声明一个大小为64的char数组
int a = 10086;    //声明一个int类型的变量并赋初始值10086
extern double l;
```

通过`&`操作符可以获得变量在内存中储存的地址，称为指针。若一个变量较大，占据了多个字节，得到的是变量第一个字节的地址。详见 [指针](../c-pointer) 章节。

> 在内存中分为两大区域，堆（Heap）与栈（Stack）。栈比较小，一般为16KiB（16384Byte），具体大小由操作系统决定。`C`语言的函数在执行时，会自动为函数分配栈空间，在函数执行结束后自动回收。堆很大很大，但是并不能由程序自动申请，需要程序员使用`calloc`或`malloc`手动申请（详见 [内存管理](../c-memory-manage) 章节），不会被自动回收，相应的，申请的内存需要手动释放，否则会造成内存泄漏。

一般情况下，在函数内声明的变量会被分配在栈上，因此通常不需要手动回收变量，声明的变量会随着栈空间的回收被一并回收。但是栈比较小，在声明一些大型变量（比如大型数组）时，可能会把栈可怜的空间撑爆，导致爆栈（Stack Overflow），然后程序boom。这时候，就需要向空间充裕的堆申请空间来存放这些大型变量。但是堆上的变量不会随着栈的回收而回收，如果不进行手动回收会一直留存，直到程序结束由操作系统统一清理。在堆空间使用完毕后，使用`free`函数释放空间。

```c
//C

int main(){
    int very_big_matrix[256][256] = {0};
    //爆栈了，数组大小256x256共65536元素，一个int占据4字节，这个数组要占据262144B即256KiB,而栈一般只有16KiB
}
```

## 数据类型

不同数据类型在内存中占用的空间大小不尽相同。`C`中的数据类型如下

### 基本数据类型

基本数据类型是算数类型，可以参与各种运算。

1. 整形。整形存在上限，超出上限将导致溢出。非`unsigned`整形最高位为符号位。

|类型|存储大小/Byte<br>(Windows_x64)|值范围<br>（Windows_x64）|存储大小/Byte<br>(Linux_x86)|存储大小/Byte<br>(Linux_x64)|
|:--:|:-:|:----:|:-:|:-:|
|char|1|-128~127/0~255|1|1|
|unsigned char|1|0~255|1|1|
|signed char|1|-128~127|1|1|
|short|2|-32,768~32,767|2|2|
|unsigned short|2|0~65,535|2|2|
|int|4|-2,147,483,648<br>~2,147,483,647|4|4|
|unsigned int|4|0~4,294,967,295|4|4|
|long int|4|-2,147,483,648<br>~2,147,483,647|4|8|
|unsigned long int|4|0~4,294,967,295|4|8|
|long|4|-2,147,483,648<br>~2,147,483,647|4|8|
|unsigned long|4|0~4,294,967,295|4|8|
|long long|8|-9223372036854775808<br>~9223372036854775807|8|8|
|unsigned long long|8|0~18446744073709551615|8|8|

> **整形的溢出**<br>整形有上限和下限，其值在超出限制后会发生溢出。<br>超出上限会产生上溢，使这个数字溢出的值从此种类型的下限开始循环，直到不溢出。比如给`signed char`赋值129，超出其上限127，溢出的2会从下限-128开始向上限循环（-128 -- 1，-127 -- 2），最终值为-127。<br>超出下限与超出上限类似，只不过是从上限向下限循环。

2. 浮点型，即小数。不同浮点型，精度不同。最高位为符号位，其余部分包含指数位和小数位，长度由占用空间决定。

<Details>
<Summary>浮点型位分配</Summary>

- long double:
  - Windows: 1符号位，11指数位，52小数位
  - Linux_x64: 1符号位，63指数位，64小数位

- double: 1符号位，11指数位，52小数位

- float： 1符号位，8指数位，23小数位

</Details>

|类型|存储大小/Byte<br>(Windows_x64)|值范围<br>（Windows）|存储大小/Byte<br>(Linux_x86)|存储大小/Byte<br>(Linux_x64)|
|:--:|:-:|:----:|:-:|:-:|
|float|4|1.175494e-38<br>~3.402823e+38|4|4|
|double|8|2.225074e-308<br>~1.797693e+308|8|8|
|long double|8|2.225074e-308<br>~1.797693e+308|12|16|

### 空类型（void）

空类型包含3种情况：

- 用于函数返回值时，表示函数没有返回值
- 用于函数参数时，表示函数没有参数（非必须）
- 用于指针时，表示无类型指针，指向确切的内存地址。

```c
//C

void func1();
int get_val(void);
void* ptr;
```

### 枚举类型

枚举类型同样是算术类型，不同的是它们只能被赋予特定的离散值。

```c
//C

enum Color {
    RED,
    GREEN = 2,//可以为枚举项赋自定义的值
    BLUE
};
//默认情况下，枚举项的值为上一项的值+1，第一项为0

enum Color myColor = RED;//声明枚举类型变量
```

### 派生类型

包括数组类型、指针类型和结构体类型。所有类型都有对应的指针类型派生，包括指针自身。

详见 [数组](../c-array)、[结构体与共位体](../c-struct-and-union)、[指针](../c-pointer)章节

## 数据类型转换

很多时候，我们需要对数据类型进行转换。`C`中的类型转换分为两种：

- 隐式类型转换

- 显示类型转换

### 隐式类型转换

隐式类型转换通常在程序运行过程中自动发生，一般出现在不同类型数据之间进行逻辑、算数或比较运算时，不需要手动操作。C语言通常会将较低的类型转换为较高的类型以避免数据丢失（低精度数据转化为高精度数据）。

<Details>
<Summary>数据类型的高低之分</Summary>

“较高”数据类型指的是能够表示更多信息（或更精确信息）的数据类型，而“较低”的数据类型则表示的信息较少。

比如：double > float > int; long long > int > short > char

浮点可以表示整数，也能表示其他所有小数；double有效位比float更多，信息更多。

对于类而言，为了避免数据丢失，会将派生等级较高的类转换为派生等级较低的类，因为子类中可能会包含父类没有的成员或方法，都转换为子类会丢失数据。

</Details>

```c
//C

int a = 1;
float b = 22.2;
a + b;//会将a转换为float再进行计算
```

> C语言在进行除法时不会进行类型转换，即使结果可能产生小数。int类型与int类型相除，结果仍为int，导致结果不正确。对其中一个int进行类型转换即可。

```c
//C

int a = 7;
int b = 3;
a / b //结果为2
(double)a / b //显式转换，结果为2.33333333
(1.0 * a) / b //隐式转换，结果为2.33333333
```

### 显式类型转换

有些情况下，数据类型并不能隐式转换，这时候就需要用到显示转换。显示类型转使用时在变量前加上一对小括号，括号内填入想转换的类型即可将变量转换为需要的类型，同时可以打破隐式类型转换的规律。如果不能转换，编译器会抛出异常。

```c
//C

int a = 7;
int b = 3;
(double)a / b //显式转换a为double，结果为2.33333333

int c = 1234;
float d = 114.514;
int e = c + (int)d//显示转换d为int
```

> 在C中也有Python的int函数把字符串转为int该多好。其实是有的，在C的标准库stdlib.h中就提供了一系列将字符串转为数字的函数（atol、atoi、strtod），在标准库stdio.h中则提供了将各种乱七八糟的类型转换为字符串的函数（snprintf，vsprintf），也可以从字符串中提取数字（sscanf）。

## 常量

常量，是程序运行中不会也不能改变的量。`C`中分为两种常量，字面常量和const常量。

养成好习惯，常量一般使用全大写命名。

### 字面常量

所有写在源代码里的字面上的值都是字面常量。其类型可以由编译器自动推断，也可以通过前缀后缀手动注明类型。

定义字面常量使用`define`预处理器。其值会在预处理器环节被替换为定义的值。

```c
//C

0x123//十六进制
123u//无符号整数
314159E-5L

int a = 123;
double d = 13F;
char b = '\n';// 这些都是字面常量

#define HOMO 114514
//使用define定义常量，代码中所有HOMO都会被替换为114514
```

### const常量

使用const关键字可以定义类似于普通变量的常量。归根到底还是特殊的变量，如果愿意还是有办法可以修改的。

```c
//C

const int CST = 114514;
```

<Details>
<Summary>修改一个const常量</Summary>

可以直接操作对应内存来进行修改。

第一种，局部常量

```c
//C

#include<stdio.h>

int main(){
    const int TARGET = 114514;//一个const常量
    int* ptr = &TARGET;
    *ptr = 10086;
    printf("%d\n",TARGET);
    //10086
}

```

第二种，全局常量，这种会麻烦一点

```c
//C

#include<stdio.h>

const int TARGET = 114514;//一个const常量，全局的
int main(){
    int* ptr = &TARGET;
    *ptr = 10086;
    printf("%d\n",TARGET);
    //boom
}
```

然后程序水灵灵的boom了。不过`C`中有个`volatile`关键字，用于标明变量在运行期间可被隐含地改变，可以用在const上。修改一下代码。

```c
//C

#include<stdio.h>

const volatile int TARGET = 114514;//一个const常量，全局的
int main(){
    int* ptr = &TARGET;
    *ptr = 10086;
    printf("%d\n",TARGET);
    //10086
}
```

修改成功。

> 为什么会这样？<br>通过这段代码，运行，查看结果

```c
//C

#include<stdio.h>

const int GLOBAL_CONST = 1;

const volatile int GLOBAL_VOLATILE_CONST = 1;

char* GLOBAL_CONST_ADDR = "6666";

int main() {
    int LOCAL_VARIBLE = 1;
    const int LOCAL_CONST = 1;
    const volatile int LOCAL_VOLATILE_CONST = 1;
    printf("Address of Global Const:          %d\n",&GLOBAL_CONST);
    printf("Address of Global Const(Precide): %d\n",GLOBAL_CONST_ADDR);
    printf("Address of Global Volatile Const: %d\n",&GLOBAL_VOLATILE_CONST);
    printf("Address of Local Varible:         %d\n",&LOCAL_VARIBLE);
    printf("Address of Local Const:           %d\n",&LOCAL_CONST);
    printf("Address of Local Volatile Const:  %d\n",&LOCAL_VOLATILE_CONST);
}
```

```txt
Address of Global Const:          4210688
Address of Global Const(Precide): 4210692
Address of Global Volatile Const: 4206608
Address of Local Varible:         6487580
Address of Local Const:           6487576
Address of Local Volatile Const:  6487572
```

> 可以看到，普通的全局const常量和字符串常量的内存位置挨在一起，共同位于`.exe`文件的`.text`区段，这是存储程序逻辑的区段，是只读的，强行修改会导致程序暴毙（这个暴毙来自于修改的行为而不是修改的结果。可以尝试修改内存地址很小，比如0的位置，程序同样会暴毙，因为内存内这些位置是只读的）。而全局volatile常量与前二者的内存地址有较大偏差，不属于`.text`区段，因此可以修改。<br>而两个局部常量的内存地址和普通局部变量挨在一起，都位于栈上，此时修改内存就和修改普通变量一样了。

</Details>

## 左值与右值

`C`中有两类表达式：左值与右值。

- 左值：指向内存位置的表达式被称为`左值（lvalue）表达式`。左值可以出现在赋值号的左边或右边。左值主要包括变量与指针。

- 右值：存储在内存中某些地址的数值被称为`右值（rvalue）`。右值是不能对其进行赋值的表达式，可以出现在赋值号的右边，但不能出现在赋值号（等号）的左边。

变量都是左值，可以出现在赋值号左边，如变量声明并初始化。常量为右值，只能出现在赋值号右侧。出现在赋值号左侧的右值会导致编译器报错。

```c
//C

int value = 10086;//有效
114 = 514;//非法语句，报错
```

## 写在最后

一个`C`程序往往包含许多变量，一切自定义的东西都可以是变量，无论是狭义的变量、指针还是函数。毕竟，程序运行在内存上，而变量就是内存区域的名称。
