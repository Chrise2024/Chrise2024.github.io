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

`IO`（Input \& Output）操作，即输入 \& 输出操作，是程序与系统、用户交互的重要渠道。程序可以从命令行、本地文件、网络、数据库以及其他途径获取输入，并向这些地方输出内容，这些操作合称 `IO` 操作。

`C` 程序会将一切设备都视为文件，对设备的处理方式和一般文件无异。 `C` 使用文件指针来访问文件。

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
//C

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

|符号|描述|
|:-:|:-:|
|%c|格式化字符及其ASCII码|
|%s|格式化字符串|
|%d|格式化整数|
|%u|格式化无符号整型|
|%f|格式化单精度浮点数（float）|
|%lf|格式化双精度浮点数（double）|
|%o|格式化无符号八进制数|
|%x|格式化无符号十六进制数|
|%X|格式化无符号十六进制数（大写）|
|%f|格式化浮点数字，可指定小数点后的精度|
|%e|用科学计数法格式化浮点数|
|%E|作用同%e，用科学计数法格式化浮点数|
|%g|%f和%e的简写|
|%G|%f 和 %E 的简写|
|%p|用十六进制数格式化变量的地址|

格式化占位符辅助指令

|符号|描述|
|:-:|:-:|
|*|定义宽度或者小数点精度|
|-|左对齐|
|+|在正数前面显示加号( + )|
|#|在八进制数前面显示零('0')，<br>在十六进制前面显示'0x'或者'0X'<br>(取决于用的是'x'还是'X')|
|0|显示的数字前面填充'0'而不是默认的空格|
|%|'%%'输出一个单一的'%'|
|(var)|映射变量(字典参数)|
|m.n.|m 是显示的最小总宽度,n 是小数点后的位数|

```c
//C

printf("This %s is %d years old.\n","dog",4);
printf("%04d\n",12);
printf("%.2f\n",114.514);
/*
This dog is 4 years old.
0012
114.51
*/
```

> 参数列表应与格式化字符串中的占位符一一对应。参数多余占位符不会产生任何结果，但是参数少于占位符会让函数之间使用未定义的垃圾值当作参数值，造成结果混乱（未定义行为）。

> 输出浮点数时， `float` 与 `double` 可以混用 `%f` 和 `%lf`。

> 输出的缓冲区只有满足特定条件后才会向 `stdout` 传递数据，这个条件一般是向缓冲区中输入换行符。

> `printf` 对字符串结束的判断依据是字符串是否遇到结束符。如果一个字符串没有以结束符结尾，函数会在内存中顺序寻找，直到遇到结束符并将这一大堆东西视为字符串整体。

### scanf

`scanf` 用于读取输入。在调用 `scanf` 函数时，程序会阻塞，直到可以从缓冲区读取数据。

`scanf` 定义如下：

```c
//C

int scanf(const char* format, ...);
//format为格式化字符串，作为输入的模板
//...为参数列表，变量的地址

int a,b;
char c[64];
scanf("%d %s",&a,c);//此处为变量的地址（字符串无所谓，可以是c也可以是&c）

scanf("%d",b);//⚠错误，此处必须为变量的地址而不是变量，这相当于传进去了一个野指针而且高概率指向地址较小的不可访问区域，可能会炸掉程序
```

`scanf` 会根据格式化字符串解析缓冲区中的内容，然后将解析到的内容依次填入参数列表指向的内存区域中。格式化字符串的占位符同 `printf`，但是不建议加上占位符的辅助指令。

在读取时，`scanf` 会将格式化字符串与缓冲区中的数据进行匹配，并摘取出占位符区域对应的数据，存储进变量所在的内存区域。如果格式化字符串和缓冲区数据格式匹配不了（"%d,%d"的格式化字符串输入"12 34"），则会导致读取异常或者程序异常（通常表现为没有读取到数据或者程序闪退）。

使用 `scanf` 读取数据时，占位符的类型必须与变量的类型完全一致（`float` 必须使用 `%f` ， `double` 必须使用 `%lf`），否则会导致读取结果异常。在读取数据时若占位符之间用空格分隔，则会将连续任意长度的空字符（回车符'\r'、换行符'\n'、制表符'\t'、空格）视为一个空格，比如`"%d %d"可以正常读取"12  \t\r\n \n\t   \r\r       34"`。

> 在使用键盘输入时，一般只有输入换行符（或按下回车，本质上是输入换行符）后才会将数据从 `stdin` 发送到缓冲区，此时 `scanf` 才能读取。

> `scanf` 读取字符串时，若字符串数组大小不足以承载输入的字符串，会导致缓冲区溢出，轻则破坏数据重则炸掉程序。

> `scanf` 读取时不会读取最后一个占位符后的空字符，这个空字符会残留在缓冲区中。下一个 `scanf` 函数会忽略残留的空字符，直接从有效数据开始读取，但是残留的空字符尤其是换行符会影响另一个输入函数：`gets`。`gets` 会直接读取到这个换行符然后误以为字符串已经结束（~~一开始就结束了~~），然后读取到的就是一个空字符串。

### gets & puts

`gets` 、 `puts` 用于读取、输出字符串，定义如下：

```c
//C

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
//C

int getchar(void);
int putchar(int c);

char a = getchar();
putchar(a);
```

`getchar` 相当于 `scanf("%c",c)`，若读取成功则返回字符对应的 `ASCII` 编码，读取失败则返回 `EOF` 。

`putchar` 相当于 `printf("%c",c)`，若输出成功则返回字符对应的 `ASCII` 编码，输出失败则返回 `EOF` 。

## 文件IO

`C` 除了对 `stdin` 这种虚拟文件的读写，可也以读写真正的文件。使用 `fopen` 函数可以打开文件并创建文件指针，然后即可对文件指针进行读写操作。

```c
//C

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
//C

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
//C

int fprintf(FILE* stream, const char* format, ...);

FILE* fp = fopen("file.txt","w");
fprintf(fp,"%d\n",114);
fclose(fp);
```

除了是向文件输出内容，其余部分和 `printf` 完全一致。

### fscanf

`scanf` 的文件输入版本，定义如下：

```c
//C

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
//C

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
//C

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
