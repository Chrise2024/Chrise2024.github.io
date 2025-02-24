---
title: C-IO操作
description: 闻道有先后，程序有IO
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-21 18:29 +0800
nav:
  prev:
    path: posts/c/c-struct-and-union
    title: C-结构体与共用体
  next:
    path: posts/c/c-memory-manage
    title: C-内存管理
---

`IO`（Input \& Output）操作，即输入 \& 输出操作，是程序与系统、用户交互的重要渠道。程序可以从命令行、本地文件、网络、数据库以及其他途径获取输入，并向这些地方输出内容，这些操作合称 `IO` 操作。`C` 的IO操作部分定义于 [stdio.h](../std-lib/stdio.h.md) 头文件。

`C` 程序会将一切设备都视为文件，对设备的处理方式和一般文件无异。 `C` 使用文件指针来访问文件。

关于IO操作函数的详细解释见 [stdio.h](../std-lib/stdio.h.md) 。

## 命令行IO

在程序运行时，会自动打开以下三个文件以供基本的命令行输入输出。

|名称|文件指针|设备|
|:-:|:-:|:-:|
|标准输入|stdin|键盘|
|标准输出|stdout|屏幕或命令行窗口|
|标准错误|stderr|屏幕或命令行窗口|

这些文件输入、输出的背后，有一片临时存放数据的区域，称为缓冲区。从 `stdin` 输入的数据会先存入缓冲区，供程序读取；向 `stdout` 发送的输出会先送入缓冲区，达到某一条件时再传递数据给 `stdout` ； `stderr` 则不存在缓冲区，向其发送到数据会直接送入 `stderr`。

### printf

`printf` 函数是最基本的输出函数，用于将内容输出到 `stdout`，然后在命令行窗口上呈现。

`printf` 定义如下：

```c
int printf(const char* format, ...);
//format为格式化字符串，作为输出的模板
//...为参数列表，可以为空

printf("Hello\n");
printf("%04d\n",114);
printf("%8.4fd\n",114.514f);
```

`printf` 函数返回输出的总字符数。

#### 格式化字符串

`printf` 的输出即格式化输出，格式化字符串中的格式化占位符会被替换为对应格式的表达式值。这些占位符都以半角百分号 `%` 开头，每种占位符对应不同的数据类型。

[格式化占位符](../std-lib/stdio.h/#printf系)

```c
printf("This %s is %d years old.\n","dog",4);
printf("%04d\n",12);
printf("%.2f\n",114.514);
/*
This dog is 4 years old.
0012
114.51
*/
```

### scanf

`scanf` 用于读取输入。在调用 `scanf` 函数时，程序会阻塞，直到可以从缓冲区读取数据。

`scanf` 定义如下：

```c
int scanf(const char* format, ...);
//format为格式化字符串，作为输入的模板
//...为参数列表，变量的地址

int a,b;
char c[64];
scanf("%d %s",&a,c);//此处为变量的地址（字符串无所谓，可以是c也可以是&c）

scanf("%d",b);//⚠错误，此处必须为变量的地址而不是变量，这相当于传进去了一个野指针而且高概率指向地址较小的不可访问区域，可能会炸掉程序
```

### gets & puts

`gets` 、 `puts` 用于读取、输出字符串，定义如下：

```c
char* gets(char* str);
int puts(const char* str);

char a[64];
gets(a);
puts(a);
```

`gets` 相当于 `scanf("%s",str)`，若读取成功则返回 `str`，读取失败则返回 `NULL` 。

`puts` 相当于 `printf("%s\n",str)`，若输出成功则返回非负值，输出失败则返回 `EOF` 。

### getchar & putchar

`getchar` 、 `putchar` 用于读取、输出单个字符，定义如下：

```c
int getchar(void);
int putchar(int c);

char a = getchar();
putchar(a);
```

`getchar` 相当于 `scanf("%c",c)`，若读取成功则返回字符对应的 `ASCII` 编码，读取失败则返回 `EOF` 。

`putchar` 相当于 `printf("%c",c)`，若输出成功则返回字符对应的 `ASCII` 编码，输出失败则返回 `EOF` 。

## 文件IO

`C` 除了对 `stdin` 这种虚拟文件的读写，可也以读写真正的文件。使用 `fopen` 函数可以打开文件并创建文件指针（亦称文件流），然后即可对文件指针进行读写操作。

```c
FILE* fopen(const char* filename, const char* mode);
//FILE* 即文件指针
//filename为文件路径，会优先从文件所在路径寻找，或者使用绝对路径
```

`mode` 为文件的读写模式，有如下几种：

|模式|描述|
|:-:|:-:|
|r|以只读模式打开文件，文件不存在则返回 `NULL`|
|w|创建一个用于写入的空文件，如果文件已经存在会删除已有的文件再创建新文件|
|a|以追加模式打开文件，将写入的内容追加到原文件末尾，文件不存在则创建空文件|
|r+|以读写模式打开文件，可以读取也可以写入，文件不存在则返回 `NULL`|
|w+|创建一个用于读写的空文件，如果文件已经存在会删除已有的文件再创建新文件|
|a+|以追加与读取模式打开文件，文件不存在则创建空文件|

> `mode`可以附加参数 `b` 和 `t`，例如 `wb+ rt ab` 。`b` 表示以二进制模式读写， `t` 表示以文本模式读写。

若读取打开成功则返回文件指针 `FILE*` ，失败则返回 `NULL` 并将错误储存在全局变量 `errno` 中（需 `errno.h` 头文件）并使用 `strerror` 函数查看错误信息。

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE* fp = fopen("404File","rb");//一个不存在的文件
    if (fp == NULL){
        printf("%s\n",strerror(errno));
        return -1;
    }
    printf("Success\n");
    fclose(fp);
}
//No such file or directory
```

在文件使用完毕后，使用 `fclose` 关闭文件，保存修改，释放资源。

> 在对文件进行读写操作时，确保和打开文件的模式匹配。

> 在打开文件时，文件会载入文件缓冲区，读写都在这个缓冲区中进行。但是与 `stdin` 的缓冲区不同，读取文件缓冲区时不会移除已读取的数据，而是移动缓冲区中的数据游标到下一块数据的位置。通过  `fseek` 函数可以移动游标的位置。

> 在读取文件时，以 `EOF` 结束文件数据。当读取到 `EOF` 时表示文件已经读取完毕。

### fprintf

`printf` 的文件输出版本，定义如下：

```c
int fprintf(FILE* stream, const char* format, ...);

FILE* fp = fopen("file.txt","w");
fprintf(fp,"%d\n",114);
fclose(fp);
```

除了是向文件输出内容，其余部分和 `printf` 完全一致。

### fscanf

`scanf` 的文件输入版本，定义如下：

```c
int fscanf(FILE* stream, const char* format, ...);

FILE* fp = fopen("file.txt","r");
char s[64];
fscanf(fp,"%s",s);
printf("%s\n",s);
fclose(fp);
```

除了是从文件读取内容，其余部分和 `scanf` 完全一致。

### fgets & fputs

分别为 `gets` 、 `puts` 的文件读写版本，定义如下：

```c
char* fgets(char* str, int n, FILE* stream);//n为要读取的字符数（包括字符串结束符的位置，实际读取的字符数为n-1）
int fputs(const char* str, FILE* stream);

FILE* fp = fopen("file.txt","rw");
char a[64];
fgets(a,4,fp);
printf("%s\n",a);
fputs(a,fp);
fclose(fp);
```

### fgetc & fputc

分别为 `getchar` 、 `putchar` 的文件读写版本，定义如下：

```c
int fgetc(FILE* stream);
int fputc(int char, FILE* stream);

FILE* fp = fopen("file.txt","ra");
char a = fgetc(fp);
printf("%c\n",a);
fputc(a,fp);
fclose(fp);
```

> `fgetc` 如果读取到了缓冲区的结束符 `EOF` 则会返回 `EOF`。

> `stdin` 和 `stdout` 作为虚拟文件，也可以使用这些函数进行读写。

## 写在最后

IO操作是程序最基本的一环之一，因为涉及程序外部的内容，在进行IO操作时一定要做好异常处理。
