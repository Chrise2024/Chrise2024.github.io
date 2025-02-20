---
title: C-循环语句
description: 都可以复读
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-05 10:29 +0800
nav:
  prev:
    path: posts/c/c-decision-statement
    title: C-判断语句
  next:
    path: posts/c/c-pointer
    title: C-指针
--- 

有时在程序中，需要将某一部分逻辑重复许多次（比如轮询、计数、监听），这时候就需要循环语句出场了。

 `C` 中循环语句有三类： `for` 、 `while` 、 `do...while` 。

## for循环

 `for` 循环通常用于有限、循环次数已知的循环。

### for循环用法

```c
//C

for (init;condition;increment){
    ////需要执行的语句
}

//C99以前
int i = 0;//i即循环控制变量
for (i = 0;i < 5;i++){
    printf("%d\n",i);
}

//C99及以后
for (int i = 0;i < 5;i++){
    printf("%d\n",i);
}
```

 `init` 语句在循环开始前执行，用于循环初始化操作，一般用于初始化循环控制变量。在 `C99` 及更新的 `ANSI C` 标准下，可以在 `init` 语句中声明变量，其作用域在 `for` 循环内部（包括 `condition` 和 `increment` 语句）。

 `condition` 为循环进行的条件，为真时会执行循环体，为假时则结束循环。在 `init` 语句结束后即执行 `condition` 判断。

 `increment` 为一轮循环结束后的操作，在循环体结束后执行，一般用于迭代循环控制变量。

> `init` 、 `condition` 、 `increment` 均可为空，但是语句之间的分号必须保留。 `condition` 语句为真时会被视为常真。

> `init` 、 `condition` 、 `increment` 均只能使用单条语句而不能使用代码块。如果想使用多条语句可以使用括号表达式，在此处括号表达式的括号可以省略。

> `for` 后可以是单一语句或代码块，称为循环体。 `for` 括号后的第一个代码块或第一条语句会被 `for` 语句识别。当然，直接在 `for` 语句括号后加分号（即不跟随任何语句）也是可行的，即空循环体。

```c
//C

int i = 0;
int j = 10;

for (;i < 5 && j > 6;i++,j -= 2){
    printf("i=%d,j=%d\n",i,j);
}

for (;;){
    //死循环
}

for (i = 0;i < 5;i++);
    printf("%d\n",i);//不是循环体
```

### for循环控制语句

在 `for` 循环内部，可以使用一些特殊的循环控制语句。

1. `break` ，用于结束当前所在的循环。如果存在多层循环嵌套则只会结束当前层而不会影响外层。

2. `continue` ，用于立即结束当前轮循环并开始新一轮循环。

```c
//C

int i = 0;
for (i = 0;i < 20;i++){
    if (i % 2 == 0){
        continue;
    }
    printf("%d\n",i);
    if (i > 12) break;
}
```

## while循环

相比 `for` 循环， `while` 循环通常用于循环次数未知的循环或无限循环。

### while循环基本用法

```c
//C

while (condition){
    ////需要执行的语句
}
```

 `condition` 为循环进行的条件，为真时会执行循环体，为假时则结束循环。在循环开始前会进行一次判断。

> `condition` 只能使用单条语句而不能使用代码块。如果想使用多条语句可以使用括号表达式，在此处括号表达式的括号可以省略。

> `while` 后可以是单一语句或代码块，称为循环体。 `while` 括号后的第一个代码块或第一条语句会被 `while` 语句识别。当然，直接在 `while` 语句括号后加分号（即不跟随任何语句）也是可行的，即空循环体。

```c
//C

int i = 0;
int j = 10;

while (i < 5 && j > 6){
    printf("i=%d,j=%d\n",i,j);
    i++;
    j -= 2;
}

while (1){
    //死循环
}

while (i++ < 5);//此处虽然看上去像是先自增再判断，但是根据运算符规则，++在右侧，所以还是先判断再自增
    printf("%d\n",i);//不是循环体
```

### while循环控制语句

在 `while` 循环内部，可以使用一些特殊的循环控制语句： `break` 、 `continue` ，用法及作用同 `for` 。

```c
//C

int i = 0;
while (i++ < 20){
    if (i % 2 == 0){
        continue;
    }
    printf("%d\n",i);
    if (i > 12) break;
}
/*
1
3
5
7
9
11
13
*/
```

## do...while循环

 `do...while` 循环是 `while` 的孪生兄弟，基本用法基本一模一样。

### do...while循环基本用法

```c
//C

do {
    ////需要执行的语句
}while (condition);
```

 `condition` 为循环进行的条件，为真时会进入下一轮循环，为假时则结束循环。

> `condition` 只能使用单条语句而不能使用代码块。如果想使用多条语句可以使用括号表达式，在此处括号表达式的括号可以省略。

> `do` 后可以是单一语句或代码块，称为循环体。 `do` 括号后的第一个代码块或第一条语句会被 `do..while` 语句识别，而且 `do` 和 `while` 之前插入多条语句或代码块会导致编译器报错。 `do` 和 `while` 之前可以不包含任何语句或代码块（ `do` 后必须加分号），即空循环体。

```c
//C

int i = 0;
int j = 10;

do {
    printf("i=%d,j=%d\n",i,j);
    i++;
    j -= 2;
}while (i < 5 && j > 6);

do {
    //死循环
}while (1);

do;
while (i++ < 5);//空循环体
```

> 与 `while` 不同， `do...while` 会先执行一次循环体再进行判断，循环体会至少执行一次。 `while` 如果起始条件判断为假则不会执行循环体。

### do...while循环控制语句

在 `do...while` 循环内部，可以使用一些特殊的循环控制语句： `break` 、 `continue` ，用法及作用同 `for` 。

```c
//C

int i = 0;
do {
    if (i % 2 == 0){
        continue;
    }
    printf("%d\n",i);
    if (i > 12) break;
}while (i++ < 20)；
```

## goto语句

和字面意思一样， `goto` 语句允许在程序逻辑内部到处跳。使用 `goto` 语句首先需要在程序内部打上标记，标记格式很简单： `label:statement` 。使用 `goto label;` 可以让程序直接跳转到 `label` 标记的语句。

```c
//C

#include <stdio.h>

int main(){
    for (;;){
        goto mylable;
    }
    mylable:
    printf("Out of loop\n");
}
```

 `goto` 通常用来一次性跳出多层循环（对应的 `break` 语句只能跳出一层循环），也可以用来快速跳过或回溯代码逻辑。

## 写在最后

循环语句极大简化了程序中的重复逻辑，但是要注意循环控制的逻辑，避免死循环或无效循环。
