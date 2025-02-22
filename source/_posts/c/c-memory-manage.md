---
title: C-内存管理
description: Stack Overflow
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-22 22:51 +0800
nav:
  prev:
    path: posts/c/c-io-operation
    title: C-IO操作
  next:
    path: posts/c/c-preprocessor
    title: C-预处理器
---

`C` 程序基本单位是函数，在函数内声明的变量会被分配在栈上。但是栈通常情况下容量很小，容易被大型变量（如数组）撑爆导致爆栈，此时就需要去向堆申请区域来存放这些变量。`C` 对内存的管理定义于 `stdlib.h` 中。

## 申请内存

申请内存使用一下三个函数：

|函数签名|描述|
|:-:|:-:|
|`void* calloc(size_t num, size_t size)`|申请 `num` 个连续大小为 `size` 的内存区段，总大小为 `num * size` Byte|
|`void* malloc(size_t size)`|申请大小为 `size` Byte的内存区域|
|`void* realloc(void *address, size_t newsize)`|将已申请的内存 `address` 重新分配，新大小为 `newsize` Byte|

这三个函数申请的内存都会被初始化为0，以无类型指针的形式返回区域的首地址，使用时需要使用强制类型转换转为需要的指针类型。如果申请失败则返回 `NULL`。

```c
//C

int* huge_array = (int*)calloc(114, sizeof(int));
char* huge_str = (char*)malloc(10086 * sizeof(char));
int* resized_array = (int*)realloc(huge_array,514 * sizeof(int));
```

## 释放内存

申请的堆内存不会随着函数执行完毕栈回收而回收，而是需要手动回收，或者一直留存直到程序运行结束由操作系统回收。运行过程中申请的堆内存如果没有及时回收，程序会随着运行过程不断申请新内存区域，导致申请的总大小不断膨胀（表现为程序内存占用升高），称为内存泄漏。

释放申请的内存使用 `free` 函数，定义如下：

```c
//C

void free(void *address);


int* huge_array = (int*)calloc(114, sizeof(int));
//亿些操作
free(huge_array);
```

## 操作细节

### 申请结果校验

申请内存可能会因为各种奇怪的原因而失败，此时不会申请到任何空间并得到一个 `NULL`。如果把返回的 `NULL` 当成申请成功的地址进行操作会产生难以预料的后果。在申请完内存后，一定要进行校验，确认申请是否成功。

```c
//C

int* huge_array = (int*)calloc(114, sizeof(int));

if (huge_array == NULL){
    printf("Alloc Failed!\n");
    return -1;
}
//操作
```

### 所有权

`C` 在内存管理中有一个十分重要的原则：谁申请谁释放。具体来说，同一块内存的申请和释放在同级作用域内完成，不要在函数内部释放传入的堆内存指针，也不要在函数内申请内存然后作为返回值返回。

> 标准库中的函数，涉及数组输出的部分，都会让你传入一个现成的数组存储输出。

```c
//C

void func1(void* ptr){
    free(ptr);//错误
}
void* func2(){
    void* ptr = malloc(114514);
    return ptr;//错误
}
```

### 内存有效性

在使用 `free` 释放内存后，那片内存区域即失效，此时不应该再对那片内存进行读写操作。如果那片区域已经被重新分配了，但是还是按照原来的方式进行读写，同样会产生难以预料的后果。

> 留意使用申请内存的地方，注意在释放内存后有没有地方还在使用已释放的内存。

```c
//C

#include <stdio.h>
#include <stdlib.h>

typedef struct{
    int a;
    char b;
    double c;
} DataFrame;

int main(){
    DataFrame* ptr = malloc(sizeof(DataFrame));
    DataFrame df = {114,'&',114.514};
    *ptr = df;
    printf("%d\n",ptr->a);
    free(ptr);
    printf("%c\n",ptr->b);//此时ptr已经被释放
    printf("End\n");
}
```

## 写在最后

内存管理，说难也难。避免内存泄漏，防止访问已释放内存都需要处处留心。随着程序体量的增大，这种情况会变得难以避免，只能养成好习惯，多想多注意，尽力减少这种情况的发生。
