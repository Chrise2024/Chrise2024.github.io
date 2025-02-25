---
title: C-错误处理
description: NullPointerException
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-25 22:16 +0800
nav:
  prev:
    path: posts/c/c-header-and-library
    title: C-头文件与库
---

程序的运行不可能一帆风顺，会出现各种千奇百怪的问题，特别是IO操作这种涉及到外部未知环境的情况下。这些出现的问题即错误 `error`，在错误出现时，需要及时处理错误。

`C` 程序的错误大致分为两种：可恢复错误和不可恢复错误。

## 可恢复错误

这种错误发生后不会中断程序运行。`C` 不直接提供对错误的处理，但是可以通过标准库访问错误信息。在标准库中，函数的返回值会标识错误的发生，同时会将错误代码存入宏 `errno` 中，通过 `perror` 或 `strerror` 函数显示错误信息。

使用 `errno` 宏需要引入 `errno.h` 头文件。

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE* file = fopen("non_existent_file.txt", "r");
    if (file == NULL) {
        printf("Error: %s\n", strerror(errno));
        perror("Opps");
        return -1;//如果错误发生后需要终止程序，应该使主函数返回-1
        //exit(EXIT_FAILURE);//或者设置退出状态为EXIT_FAILURE
    }
    fclose(file);
    return 0;
}
/*
Error: No such file or directory
Opps: No such file or directory
*/
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>

int main() {
    int* ptr;
    ptr = (int*)malloc(114);
    if (ptr == NULL){//防止空引用
        perror("Malloc failed");
        exit(EXIT_FAILURE);
    }
    ptr[10] = 114514;
    exit(EXIT_SUCCESS);
}
```

## 不可恢复错误

这种错误一旦发生会直接导致程序异常终止，比如数字除以0、访问不可访问的地址、空引用、爆栈、数组越界、缓冲区溢出。这种错误很难察觉，会在IO操作时因为奇奇怪怪的外部输入而被引发。

对于这种错误，只能通过防范的方式解决。在进行IO操作时额外进行校验操作能最大程度避免错误的发生。

```c
//防止除以0
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>

int main() {
    int a,b;
    scanf("%d %d",&a,&b);
    if (b == 0){
        fprintf(stderr, "Error, Divid by 0\n");
        exit(EXIT_FAILURE);
    }
    printf("%d / %d = %lf\n",a,b,(double)a / b);
    exit(EXIT_SUCCESS);
}
```

## 写在最后

错误常有，遇到在可能产生错误的地方一定要设置处理错误的分支，及时处理错误。
