---
title: C-数组
description: 醒醒，你的数组越界了
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-18 17:12 +0800
nav:
  prev:
    path: posts/c/c-function
    title: C-函数
  next:
    path: posts/c/c-string
    title: C-字符串
---

数组是固定大小的同种元素的集合，其内部的元素在内存上连续、顺序存储，每个元素相对第一个元素的位置值称为索引（第一个元素索引为0，第二个元素索引为1，以此类推）。可以通过索引访问并修改数组中的元素。

## 声明数组

```c
//C

variable_type array_name[size];//size为数组大小，可以是变量或常量，必须为常量或常量表达式
int array1[5];//数组内元素为int类型
char str[64];//数组内元素为char类型
double* array2[12];//数组内元素为指向double的指针
```

## 数组的初始化

在数组声明完毕后，编译器会为数组分配内存空间，大小为数组占用的总大小，即 `size * sizeof(variable_type)` 。分配规则遵循一般变量的内存分配规则 。但是在分配空间后，编译器不会对分配的空间做进一步处理，内部可能存在遗留的垃圾数据，这时候需要对数组进行初始化。数组初始化可以使用初始化器，即使用 `{}` 表达式， `{}` 内部即为要填入数组的初始元素，之间用逗号隔开；也可以手动填入数据。

```c
//C

int array[5] = {1,2,3,4,5};//{}内的数据会从前往后依次填入数组

int array1[5] = {1,2,3};//若{}内元素数目少于数组大小，缺少的部分会被初始化为0值

int array2[5] = {1,2,3,4,5,6};//若{}内元素数目多于数组大小，多余的数据会被忽略

int array3[5] = {[2] = 4,[3] = 10,8}//可以通过 `[index] = ` 指定将数据填入的位置，如果后续扔存在非定位数据则会从初始化器中最后指定的位置开始填充。

int array4[] = {1,2,3,4};//若未指定数组大小，编译器会根据初始化器自动推断数组大小，此时数组大小为4

int array5[3];
array5[0] = 1;
array5[1] = 2;
array5[2] = 3;//手动初始化
```

> 使用初始化器初始化数组时，会在编译器的编译阶段被转换成和手动初始化一样的形式（通过汇编指令填入数据）。本质上来说，两种初始化没有区别，有的只是代码整洁度的差异。

> 切记，数组默认分配栈空间，数组过大会导致爆栈，程序boom。

## 数组元素的访问

访问数组中的元素通过访问运算符 `[]` ，以元素的索引为标志： `array[index]` 。

```c
//C

int array[5] = {0};
array[0] = 10086;
printf("%d\n",array[0]);
//10086
```

> 仅声明时使用常量或常量表达式声明的数组允许使用初始化器进行初始化。

## 数组指针

数组指针即指向数组的指针。在声明一个数组时，对应的变量本质上就是指向数组第一个元素的指针。任意与数组元素同类型的指针都可以等效为一个数组，但是会缺失数组大小这一信息。

```c
//C

int array[5] = {0};
int* ptr;
ptr = array;
printf("%d\n",ptr[0]);
```

> **指针偏移**<br>数组元素的访问本质上是通过指针进行，因为元素是连续存储的，所以对数组首元素的地址偏移对应大小即可获得相应元素的地址。这种操作可以之间对数组指针加一个整数实现。 `C` 的数组指针在进行加（减同理）操作时，会按照数组元素的大小进行偏移，而不是一个字节一个字节的偏移。比如 `int` 类型的数组，数组指针每 `+1` ，指针的位置会移动四个字节。

```c
//C

#include <stdio.h>

int main() {
    int array[5] = {1,2,3,4,5};
    int i = 0;
    for (;i < 5;i++){
        printf("Address from array access  :%d\n",&array[i]);
        printf("Address from pointer offset:%d\n",array + i);
    }
}
/*
Address from array access  :6487552
Address from pointer offset:6487552
Address from array access  :6487556
Address from pointer offset:6487556
Address from array access  :6487560
Address from pointer offset:6487560
Address from array access  :6487564
Address from pointer offset:6487564
Address from array access  :6487568
Address from pointer offset:6487568
*/
```

> 通过 `[]` 访问数组时，会被编译器解析为指针偏移再通过 `*` 运算符访问指针所指的变量。 `array[index]` 会被解析为 `*(array+index)` 。基于此种处理，可以通过 `index[array]` 这种奇怪的方式访问数组的元素。这种方式会被解析为 `*(index+array)` ，可以正常偏移指针，从而正常访问。

```c
//C

#include <stdio.h>

int main() {
    int array[5] = {1,2,3,4,5};
    3[array] = 10086;
    printf("%d\n",3[array]);
}
//10086
```

## 数组切片

数组和指针可以互用，如果一个指针指向数组中间的某个元素，那么这个指针就相当于原数组从那个元素开始一直到结尾所以元素的切片。更多情况下，这种单向切片用于字符串中。

## 多维数组

 `C` 支持多维数组。其一般声明如下，元素的访问类似一维数组。

```c
//C

variable_type array_name[size1][size2]...[sizeN];

int matrix[3][3] = {0};
int threedimarray[5][5][5] = {0};

matrix[1][1] = 114;
threedimarray[2][2][2] = 514;
```

> 在使用初始化器初始化多维数组时，编译器会按 `深度优先` 原则填入数据。本质上，这也是多维数组在内存上的存储方式，初始化器会按内存的先后顺序填入数据。

```c
//C

#include <stdio.h>

int main() {
    int threedimarray[2][2][2] = {0,1,2,3,4,5,6,7};
    printf("%d\n",threedimarray[0][0][0]);
    printf("%d\n",threedimarray[0][0][1]);
    printf("%d\n",threedimarray[0][1][0]);
    printf("%d\n",threedimarray[0][1][1]);
}
/*
0
1
2
3
*/
```

## 交错数组

多维数组每个维度大小都是固定的（这与存储方式有关）。如果需要每个维度大小不一则需要交错数组。

交错数组本质上是元素类型为指针的数组，通过指针指向不同的位置的数组从而摆脱内存存储的限制。

```c
//C

int* interleaved_array[3];
int array1[3] = {0};
int array2[4] = {0};
int array3[5] = {0};
interleaved_array[0] = array1;
interleaved_array[1] = array2;
interleaved_array[2] = array3;
```

## 数组作为函数参数

可以向函数传入数组，函数声明与定义如下。

```c
//C

void print_array1(int[],int);

void print_array2(int*,int);

void print_array1(int array[],int size){
    int i = 0;
    for (;i<size;i++){
        printf("%d\n",array[i]);
    }
}
void print_array2(int* array,int size){
    int i = 0;
    for (;i<size;i++){
        printf("%d\n",array[i]);
    }
}
```

## 从函数返回数组

一般声明的数组会在栈上分配空间，会随着栈回收而回收，传出的数组也会因此无效。此时可以通过将数组声明为 `static` （受 `static` 的限制，只能返回特定大小的数组）或手动申请堆内存来摆脱栈回收的限制（详见 [内存管理](../c-memory-manage) 章节）。但是十分不建议这样做，因为违反了 `谁申请谁回收` 的原则，由函数申请的内存必须由同一个函数释放。

```c
//C

#include <stdlib.h>

int* gen_array(int size){
    int* array = (int*)malloc(size * sizeof(int));
    int i = 0;
    for (;i<size;i++){
        array[i] = i;
    }
    return array;
}
```

在涉及函数返回数组时，正确做法是向函数传入一个数组，函数对这个数组处理后再将其返回（由于已经处理完数组了，返不翻会都一样）。观察可以发现，标准库中的函数都是要求传入数组。

## 数组越界

数组在声明时，编译器会记录数组的大小。当访问数组的索引超出数组大小时，会导致数组越界并抛出异常。但是使用指针指向数组后，数组的大小信息会丢失，导致越界检测失效，此时如果数组越界，会导致意想不到的情况。

## 写在最后

数组是除了基本数据类型以外最重要的类型之一，用处很多。在使用数组时，谨防数组越界以及大数组爆栈。
