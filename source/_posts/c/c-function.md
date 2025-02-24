---
title: C-函数
description: 我是三角函数，你又是什么函数
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-09 15:48 +0800
nav:
  prev:
    path: posts/c/c-pointer
    title: C-指针
  next:
    path: posts/c/c-array
    title: C-数组
---

函数是 `C` 语言程序的基本组成部分之一，程序的入口点----主函数就是一个典型的函数。

函数是一组语句的集合，用来执行特定操作，可以有参数决定内部逻辑，也可以再执行完毕后返回一个值。函数可以将复杂的操作封装在一起，避免直接接触内部的业务逻辑，只用关注函数的功能。

## 函数的声明

函数在调用前必须声明，基本格式如下：

```c
return_type function_name(parameters);
//return_type为函数的返回值，可以是任意变量类型，在没有返回值时则为void
//function_name为函数名，标准标识符名称
//parameters为参数，单个参数类似变量声明，参数之间用逗号隔开，无参数则留空或void

void func1();
int main(int argc,char** argv);
double greet(void);//返回double，无参数
char homo(int,float,int*);//在声明函数时，参数名可以省略
```

> **函数签名**<br>函数的签名由返回值和参数（数量、类型、顺序）共同决定，是一个函数最重要的标识之一。不同名的函数可以有相同的签名，具有相同签名的函数的指针是通用的。

> **参数**<br>参数在调用函数时从函数外部传入，供函数内部使用。外部传入的值称为实际参数（实参），函数内部使用的参数为形式参数（形参）。在向函数传递参数时，传入的实际上是参数的拷贝，而不是参数本身。在函数内对形参的修改不会影响实参。

## 函数的定义

声明完的函数只是一个空壳，没有任何实际作用，只是告诉编译器这个函数是存在的。在声明完函数后，需要定义这个函数，为函数补上函数体。

```c
return_type function_name(parameters){
    //函数体
}

int add(int a,int b){
    return a+b;
}
```

> 函数的声明和定义可以同时进行，但是必须在代码中调用的位置之前。

## 调用函数

在函数完成声明和定义后就可以调用了。

```c
#include <stdio.h>

int add(int a,int b){
    return a+b;
}

void greet(int number){
    printf("Hello,No.%d\n",number);
}

int main() {
    int result = add(1,2);
    printf("1+2=%d\n",result);
    greet(result);
}
//1+2=3
//Hello,No.3
```

> 函数存在返回值时，整个被调用函数为返回值类型的右值。

## 函数的传参

函数在被调用时需要传入参数。传入的参数为实参，函数内部使用的为形参，而形参为实参的拷贝，因此对形参的修改不会影响实参。如果需要在函数内修改传入变量，则需要传入指向变量的指针。尽管指针也是拷贝，但是指针指向的变量是确定的（一个人的名片可以复制，但是人还是那个人），从而修改变量。

```c
#include <stdio.h>
#include <stdlib.h>

void modify_var(int* var1,int var2){
    *var1 = 114;
    var2 = 514;
}

int main() {
    int v1 = 1;
    int v2 = 2;
    modify_var(&v1,v2);
    printf("After:\n");
    printf("v1 = %d\n",v1);
    printf("v2 = %d\n",v2);
}
/*
After:
v1 = 114
v2 = 2
*/
```

## 函数的返回值

函数可以通过 `return` 返回一个返回值。

```c
#include <stdio.h>

int add(int a,int b){
    return a+b;
}

int compare(int a,int b){
    if (a > b){
        return 1;
    }
    else if(a == b){
        return 0;
    }
    else{
        return -1;
    }
}

void greet(int number){
    if (number < 0){
        printf("Hello,No.%d\n",-number);
        return;
    }
    printf("Hello,No.%d\n",number);
}
```

使用 `return` 返回值后，函数即结束执行，后续所有语句都会被忽略。

> 在函数无返回值（返回值为 `void` ）的情况下，依然可以使用 `return` 终止函数的执行。

> 如果有返回值的函数在执行完所有语句后未通过 `return` 返回值，会导致未定义行为。

## 函数指针

函数作为程序的一部分，自然是在内存上。既然在内存上，那么就可以有指针指向函数。指向函数的指针称为函数指针，可以指向有对应签名的函数。

```c
return_type (*function_ptr)(parameters);//声明一个函数指针
//return_type为函数的返回值
//function_ptr为指针名
//parameters为函数参数列表，参数只有类型，参数之间用逗号隔开，无参数则留空或void

int (*main_ptr)(int,char**);//指向具有此类签名的函数，如 int main(int argc,char** argv)
```

> **函数指针声明太麻烦怎么办**<br>函数指针声明很复杂，在需要多次使用时会很麻烦，此时可以使用 `typedef` 给函数指针取一个别名。

```c
typedef void (*my_function)();

my_function func_ptr;
void (*funcptr)();
//二者等价
```

在声明函数指针后，可以之间将函数赋值给函数指针，之后可以像使用正常函数一样使用函数指针，函数指针会被视为具有相应签名的普通函数。

```c
#include <stdio.h>

void say_hello(int a){
    printf("Hello,%d\n",a);
}

int main() {
    void (*hello_copy)(int) = /*我是函数*/say_hello;
    hello_copy(1234);
}
```

> 所有具有相同签名的函数都可以赋值给对应类型的函数指针。当函数签名与函数指针指针不匹配是，编译器会抛出警告，但是仍可以成功编译。这种情况下，在使用函数指针时，传入的参数与返回值会按函数指针对应的签名解析（对参数进行隐式类型转换），导致原函数收到的参数货不对板，导致传参异常或者其他未定义行为。

## 函数嵌套

在 `GCC` 编译器下，允许在函数体内部声明&定义函数，可以达成闭包。

> `MSVC` 与 `LLVM` 编译器均禁止这种行为。

```c
void func() {
    printf("Hello,func\n");
    int tmp = 555;
    void inner_func()
    {
        printf("Hello,inner func\n");
    }
    inner_func();
}
```

内层函数在没有调用栈上的变量是会被分配在 `text` 区段。若内层函数调用了栈上的变量（比如外层函数内声明的变量）则会被分配到栈上。

## 写在最后

函数能消除重复代码，简化程序逻辑，即组成程序，又强化代码组织。
