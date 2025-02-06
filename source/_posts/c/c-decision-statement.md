---
title: C-判断语句
description: if else
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-04 11:08 +0800
nav:
  prev:
    path: posts/c/c-scope
    title: C-作用域
  next:
    path: posts/c/c-loop-statement
    title: C-循环语句
---

# 判断语句

在程序逻辑中，经常会遇到一些不同的条件并根据条件进行不同的操作，这里便是判断语句的业务范围。

`C`中判断语句分为两类：`if`语句和`switch`语句。

## if语句

`if`语句是最基本的判断语句，基本格式如下：

```c
//C

if (condition){
    //需要执行的语句
}

if (condition){
    //需要执行的语句
}
else if (condition){
    //需要执行的语句
}
else{
    //需要执行的语句
}

if (condition){
    //需要执行的语句
}
else{
    //需要执行的语句
}
```

`condition`为条件表达式，决定语句的执行逻辑。`condition`为真则执行if后的语句或代码块。`if`语句可以独立存在，也可以在后面附加`else if`进行连续判断，或者在最后加上`else`作为条件未命中的操作。一个`if`语句可以包含任意数量的`else if`以及至多一个`else`且必须出现在最后。

> `C`没有布尔类型表示逻辑真假。`C`会将所有非0或非`NULL`的值视为真，0或`NULL`视为假。

> `if`、`else if`、`else`后的语句可以是单一语句，也可以是代码块。只有条件语句括号后的第一个代码块或第一条语句会被判断语句识别。当然，直接在条件语句括号后加分号（即不跟随任何语句）也是可行的。

```c
//C

int condition = 0;
if (condition)
printf("Hello\n");//第一个printf属于这个if语句，第二个printf则不属于
printf("World\n");
if (0);//永远为假的condition，但是后面直接加了分号，相当于空语句
printf("Aha?\n");
/*
 * World
 * Aha?
 */
```

## switch语句

`switch`语句用于对条件的多种情况分别进行匹配，基本语法如下：

```c
//C

switch(condition){
    case <constant_expr1>://可以有不限数量个case
        //需要执行的语句
    case <constant_expr2>:
        //需要执行的语句
        break;
    default://可选，非必须
        //blblbl
}
```

`condition`为条件，`switch`会从上到下一次把`condition`和`case`后的常量表达式比较，如果相同则命中，执行对应`case`下的语句，如果所有`case`均未命中则执行`default`中的语句（如果存在）。在`case`的语句中可以用`break`来终止语句的执行。

> **常量表达式**<br>`case`的条件必须是常量表达式，即字面常量或仅由字面常量组成的表达式（静态变量与const常量均不行），如`10086`、`1+1`、`114*514`、`2/1`。

> **`case`的击穿**<br>在`switch`语句中，如果一个`case`下的语句未以`break`结尾，那么在运行完这个`case`后程序会继续执行后续所有`case`下的语句并无视`case`的条件，直到运行完所有`case`（default的语句也不能幸免）或者遇到`break`。

```c
//C

#include<stdio.h>

int main(){
    switch(1){
    case 1://会命中这个case
		printf("1\n");
		
    case 2:
		printf("2\n");
        
    default:
		printf("3\n");
    }
}
/*
 * 1
 * 2
 * 3
 */
```

## ? :运算符

这也是一种“判断语句”。基本格式如下：

```c
//C

//condition ? expr1 : expr2

int a = 114514;
int b = a > 10086 ? 114 : 514;
a % 2 ? printf("even\n") : printf("odd\n");
```

在`condition`为真时，会执行`expr1`并将其值作为整个表达式的值，`expr2`被忽略；`condition`为假时，会执行`expr2`并将其值作为整个表达式的值，`expr1`被忽略。

# 写在最后

判断语句能极大增加程序的灵活性以应对不同的情况。