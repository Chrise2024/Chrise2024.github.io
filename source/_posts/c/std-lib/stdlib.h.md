---
title: C标准库-stdlib.h
description: stdlib.h
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-24 21:19 +0800
---

`stdlib.h` 提供标准库函数。

## 类型

1. `size_t`，为 `sizeof` 的返回值，是 `unsigned long long` 的别名。

2. `div_t` ，为 `div`函数的返回值。

3. `ldiv_t`，为 `ldiv`函数的返回值。

## 宏

1. `NULL`，表示空指针，值为 `0LL`。

2. `EXIT_FAILURE`，`exit` 函数失败时的返回值，值为-1。

3. `EXIT_SUCCESS`，`exit` 函数成功时的返回值，值为0。

4. `RAND_MAX`，`rand` 返回的最大值。

5. `MB_CUR_MAX`，多字符集中最大字符数。

## 函数

### 转换函数

1. `double atof(const char *str)`

将字符串转换为双精度浮点，转换失败则返回 `0.0`。

```c
char str[] = "114.514";
double f = atof(str);
```

2. `int atoi(const char *str)`

将字符串转换为整形，转换失败则返回 `0`。

```c
char str[] = "114514";
int f = atoi(str);
```

3. `long atol(const char *str)`

将字符串转换为长整型，转换失败则返回 `0`。

```c
char str[] = "114514";
long f = atol(str);
```

4. `double strtod(const char *str, char** endptr)`

尝试将字符串前尽可能多的位转换为双精度浮点数，并将指向不可转换部分首位的指针存入 `endptr`，转换失败则返回 `0`。

```c
char str[] = "114.514Hello";
char* endptr;
double f = strtod(str,&endptr);
puts(endptr);
// Hello
```

5. `long int strtol(const char *str, char **endptr, int base)`

尝试将字符串前尽可能多的位按 `base` 进制转换为长整型，并将指向不可转换部分首位的指针存入 `endptr`，转换失败则返回 `0`。

```c
char str[] = "1E2D3CHello";
char* ptr;
long f = strtol(str,&ptr,16);
printf("%d - %s\n",f,ptr);
// 1977660 - Hello
```

5. `unsigned long int strtoul(const char *str, char **endptr, int base)`

尝试将字符串前尽可能多的位按 `base` 进制转换为无符号长整型，并将指向不可转换部分首位的指针存入 `endptr`，转换失败则返回 `0`。

```c
char str[] = "1E2D3CHello";
char* ptr;
unsigned long f = strtoul(str,&ptr,16);
printf("%u - %s\n",f,ptr);
// 1977660 - Hello
```

### 内存管理

1. `void* calloc(size_t num, size_t size)`

申请 `num` 个连续大小为 `size` 的内存区段，总大小为 `num * size` Byte，返回指向这个区段的指针。

```c
int* huge_array = (int*)calloc(114, sizeof(int));
```

2. `void* malloc(size_t size)`

申请大小为 `size` Byte的内存区域，总大小为 `num * size` Byte，返回指向这个区域的指针。

```c
char* huge_str = (char*)malloc(10086 * sizeof(char));
```

3. `void* realloc(void* address, size_t newsize)`

将已申请的内存 `address` 重新分配，新大小为 `newsize` Byte，返回指向新区域的指针。

```c
int* resized_array = (int*)realloc(huge_array,514 * sizeof(int));
```

4. `void free(void *ptr)`

释放使用 `calloc`、`malloc` 或 `realloc` 申请的内存。

### 系统函数

1. `void abort(void)`

立即使程序异常终止，并在系统允许的情况下生成一个核心转储文件。

2. `int atexit(void (*func)(void))`

在程序正常终止时调用相应函数，不包括异常终止。

```c
#include <stdio.h>
#include <stdlib.h>

void nexit(){
    printf("Exited\n");
}

int main() {
    atexit(nexit);
    printf("Finish\n");
}
```

3. `void exit(int status)`

使程序以指定状态正常终止，一般使用宏 `EXIT_FAILURE` 和 `EXIT_SUCCESS` 指定状态。

```c
#include <stdio.h>
#include <stdlib.h>

void nexit(){
    printf("Exited\n");
}

int main() {
    atexit(nexit);
    exit(EXIT_SUCCESS);
    printf("Finish\n");
}
```

4. `char *getenv(const char *name)`

返回 `name` 名称对应的环境变量的值。

```c
puts(getenv("Path"));
```

5. `int system(const char *string)`

将指令交给操作系统的命令行执行，返回执行状态，出现错误则返回-1。

```c
system("echo HelloWorld");
```

### 数组操作

1. `void *bsearch(const void *key, const void *base, size_t nmemb, size_t size, int (*compar)(const void*, const void*))`

在数组 `base` 中使用二分查找寻找与 `key` 指向的数据匹配的值，返回指向该数据的指针，未找到则返回 `NULL`。

> `nmemb` 为数组大小。

> `size` 为数组元素的大小， `key` 与 `base` 必须为同一类型。

> `compar` 为比较方法，函数会以这个函数的返回值为比较相等的依据，传入的函数必须具有 `int (const void *a, const void *b)` 签名，`int (void*, void*)` 亦可。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct{
    char name[64];
    int number;
} Book;

int custom_cmp(void* a,void* b){
    Book* pb1 = (Book*)a;
    Book* pb2 = (Book*)b;
    return strcmp(pb1->name,pb2->name) && pb1->number - pb2->number;
}

int main() {
    Book books[] = {
        {"C Primer Plus",10086},
        {"Mathematical Analysis",114514},
        {"Linear Algebra",1919810},
        {"Journey to the West",1437}
    };
    
    Book target = {"Mathematical Analysis",114514};
    Book* find = (Book*)bsearch(&target,books,4,sizeof(Book),custom_cmp);
    
    if (find == NULL){
        printf("Book not found\n");
        return -1;
    }
    printf("Found book: %s - %d\n",find->name,find->number);
    return 0;
}
```

2. `void qsort(void *base, size_t nitems, size_t size, int (*compar)(const void*, const void*))`

对数组 `base` 进行快速排序。

> `nmemb` 为数组大小。

> `size` 为数组元素的大小。

> `compar` 为比较方法，函数会以这个函数的返回值为比较大小的依据，传入的函数必须具有 `int (const void*, const void*)` 签名，`int (void*, void*)` 亦可。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void print_array(int* array,int cnt){
    int i = 0;
    for (;i < cnt;i++){
        printf("No.%2d - %d\n",i,array[i]);
    }
}

int custom_cmp(void* a,void* b){
    int* pa = (int*)a;
    int* pb = (int*)b;
    return *pa - *pb;
}

int main() {
    int arr[] = {8,2,4,5,6,7,8,4,6,8,4,8,0,4,3,8,5,7};
    printf("Before Sort:\n");
    print_array(arr,sizeof(arr) / sizeof(int));
    printf("=======================================\n");
    printf("Sorted:\n");
    qsort(arr,sizeof(arr) / sizeof(int),sizeof(int),custom_cmp);
    print_array(arr,sizeof(arr) / sizeof(int));
}
```

### 数学函数

1. `int abs(int x)` & `long labs(long x)`

返回 `x` 的绝对值。

2. `div_t div(int numer, int denom)` & `ldiv_t ldiv(long int numer, long int denom)`

返回 `number` 除以 `denom` 的结果，分别用 `div_t` 和 `ldiv_t` 表示，二者定义如下：

```c
#ifndef _DIV_T_DEFINED
#define _DIV_T_DEFINED

  typedef struct _div_t {
    int quot;
    int rem;
  } div_t;

  typedef struct _ldiv_t {
    long quot;
    long rem;
  } ldiv_t;
#endif
```

其中 `quot` 为商， `rem` 为余数，在除数或被除数小于0时向0舍入。

3. `int rand(void)`

返回一个介于0和 `RAND_MAX` 之间的伪随机数，常搭配 `srand` 使用。

4. `void srand(unsigned int seed)`

设置 `rand` 使用的随机数发生器的种子，一般使用当前时间戳作为种子。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

int main() {
    int i = 0;
    time_t t;
    srand((unsigned) time(&t)/* 获取时间戳 */);
    for (;i < 10;i++){
        printf("%d\n",rand());
    }
}
```
