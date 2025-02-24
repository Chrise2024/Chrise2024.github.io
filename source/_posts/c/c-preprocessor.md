---
title: C-预处理器
description: 预制代码说是
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-23 18:29 +0800
nav:
  prev:
    path: posts/c/c-memory-manage
    title: C-内存管理
  next:
    path: posts/c/c-header-and-library
    title: C-头文件与库
---

预处理器用于在编译之前处理源代码文本、条件选择、设置编译选项。预处理器指令以 `#` 开头，由自已的语法格式在编译器完成预处理后会被去除。

## 条件选择

预处理器中的条件语句可以实现根据特定情况启用特定语句，包括 `C` 代码和其他预处理器。

|指令|描述|
|:-:|:-:|
|`#ifdef`|检查宏是否已定义，已定义则编译下面的代码|
|`#ifndef`|检查宏是否未定义，未定义则编译下面的代码|
|`#if`|如果给定条件为真，则编译下面的代码|
|`#else`|上述三个 `if` 判断为假时则编译下面的代码|
|`#elif`|上述三个 `if` 判断为假时再次进行判断|
|`#endif`|结束条件块，任何一个 `if` 或 `else` 指令都需要 `#endif` 封闭|

```c
#ifdef __WIN64__
#define IsWindows64 1
#else
#define IsWindows64 0
#endif

#if __STDC__
printf("Hello,ANSI C\n");
#endif

#ifndef May_Be_Defined_Macro
#define May_Be_Defined_Macro
#endif
```

## 文本处理

顾名思义，这一类预处理器直接对源代码进行操作，生成处理过的低级源码交给后续步骤。

### include

在程序源码的开头，见的最多的就是 `#include <xxx>` 。这一预处理器指令用于导入外部文件，并将整个预处理器指令替换为文件所有内容。这个文件可以是任意文件，不只局限于拓展名为 `.h` 的头文件，但是习惯上导入的都是 `.h` 拓展名。

假设有以下代码

```c
//header.h
int const_var = 114514;
void greet(void);


//main.c
#include <header.h>

int main(){
    //...
}
```

在预处理后会变成

```c
int const_var = 114514;
void greet(void);

int main(){
    //...
}
```

> 可以通过 `gcc` 编译器的 `-E` 选项只进行预处理而不进行后续步骤。

> 导入文件分为 `<file>` 和 `"file"` 两种形式。其中 `<file>` 用于导入标准库文件，编译器只会在标准库目录中寻找文件； `"file"` 用于导入自定义文件，编译器会首先在源文件所在目录寻找文件，然后再去标准库目录寻找文件。如果导入的文件不存在编译器会报错。

### define

`define` 指令又称宏，用于定义文本替换。

宏不能重复定义，否则编译器会抛异常。宏的重复定义常发生在导入多个头文件时（比如 `string.h` 和 `stdio.h` 均定义了 `NULL`），为解决冲突问题，这些头文件使用了 `#ifndef` 指令确保宏不会被重复定义。

```c
//stdio.h对NULL的定义，此处为了易于阅读做了缩进处理
#ifndef NULL//如果先前未定义NULL才会定义NULL
    #ifdef __cplusplus
        #ifndef _WIN64
            #define NULL 0
        #else
            #define NULL 0LL
        #endif  /* W64 */
    #else
        #define NULL ((void *)0)
    #endif
#endif
```

#### define-常量

`define` 可以用来定义字面常量。

```c
#define HOMO 114514
#define PI 3.14159
#define Empty//可以定义一个空替换，相当于直接忽略这个token
```

在上述代码中，预处理器会将源代码里的所有 `HOMO` Token替换为114514， `PI` Token替换为3.14159，从而达成常量的效果。

#### define-宏函数

`define` 指令除了可以实现常量，也可以实现宏函数。

```c
#define square(x) x * x
```

这个宏会将所有 `square(x)` 替换为 `x * x` ，`x`为任意内容。因为只是文本替换，预处理器不会检查运算的合法性。

> 同样是因为文本替换，如果x是一个表达式，那么在进行替换时可能会影响到原有的运算符优先级，导致错误的结果。

```c
#define square(x) x * x

printf("%d\n",square(3 + 2));//预期是25
//11
//此处被替换为 3 + 2 * 3 + 2 ，结果被运算符优先级影响了
```

在解析宏函数时， `#define square(x)`后的内容会以空格或 `##` 为分隔符分为若干个Token。Token中的 `x` 会被替换为宏函数对应的参数，空格会被原样还原（无论多少空格均只留下一个空格）， `##` 会被完全去除。

```c
#define mkident(s) prefix##s suffix    lololo
/* ... */
int mkident(int) = 0;

//预处理后如下
int prefixint suffix lololo = 0;
```

#### define-函数别名

`define` 指令还可以给函数取别名，将统一的函数名称根据环境重定向到适用于不同环境有不同名称的函数上；或者给函数填充参数，实现预制函数

```c
//这段代码来自 winuser.h(windows.h)的一部分，根据环境启用不同的函数定义
#ifdef UNICODE
#define MessageBox  MessageBoxW
#else
#define MessageBox  MessageBoxA
#endif // !UNICODE

#define print_int(x) printf("%d\n",x)//预制函数
print_int(114514);
```

#### undef-取消宏

在定义宏后，可以使用 `undef` 取消宏定义，此时这个宏就相当于不存在了。

### 预定义宏

`C` 的编译器默认提供一些已经定义好的宏，用于获取编译时的环境信息。这些宏中一部分由 `ANSI C` 标准定义，一些由编译器自行定义。

#### ANSI宏

这些宏由 `ANSI C` 标准定义，在任何使用 `ANSI C` 标准的编译器下都有效

|宏|描述|
|:-:|:-:|
|\_\_DATE\_\_|当前日期，一个以 `"MMM DD YYYY"` 格式表示的字符常量|
|\_\_TIME\_\_|当前时间，一个以 `"HH:MM:SS"` 格式表示的字符常量|
|\_\_FILE\_\_|含当前文件名，一个字符串常量|
|\_\_LINE\_\_|当前行号，一个十进制整数常量|
|\_\_FUNCTION\_\_|当前函数名，一个字符串常量|

#### GCC宏

这些宏由 `gcc` 编译器定义，仅在 `gcc` 编译环境下有效。

由于定义的宏数量十分巨大，此处仅列举常用的。可以使用 `echo | gcc -dM -E -` 查看所有 `gcc` 定义的宏。

1. 编译器版本相关

|宏|描述|
|:-:|:-:|
|\_\_GNUC\_\_|`gcc` 主版本号，十进制整数常量|
|\_\_GNUC_MINOR\_\_|`gcc` 副版本号，十进制整数常量|
|\_\_GNUC_PATCHLEVEL\_\_|`gcc` 修订版本号，十进制整数常量|

`__GNUC__.__GNUC_MINOR__.__GNUC_PATCHLEVEL__` 即 `gcc` 版本（如4.9.2），可以使用 `gcc --version` 查看。

2. 操作系统相关

在不同操作系统下进行编译或者设置编译目标平台时，相应的宏会被定义为1，否则无定义。

|操作系统|宏|
|:-:|:-:|
|Windows|\_\_WIN32\_\_, \_\_WIN64\_\_, \_WIN32, \_WIN64|
|Linux|\_\_linux\_\_, linux, \_\_gnu_linux\_\_|
|macOS|\_\_APPLE\_\_, \_\_MACH\_\_|
|Unix|\_\_unix\_\_, \_\_unix|
|Android|\_\_ANDROID\_\_|
|FreeBSD|\_\_FreeBSD\_\_|
|OpenBSD|\_\_OpenBSD\_\_|
|NetBSD|\_\_NetBSD\_\_|

3. 架构相关

用于识别CPU架构，不同架构下相应的宏会被定义为1，否则无定义。

|CPU架构|宏|
|:-:|:-:|
|x86（32-bit）|\_\_i386\_\_, _M_IX86|
|x86_64（64-bit）|\_\_x86_64\_\_, \_M_X64|
|ARM 32-bit|\_\_arm\_\_, \_\_thumb\_\_, _M_ARM|
|ARM 64-bit (AArch64)|\_\_aarch64\_\_|
|PowerPC|\_\_powerpc\_\_, \_\_powerpc64\_\_|
|MIPS|\_\_mips\_\_|
|RISC-V|\_\_riscv|

4. 语言标准

|宏|描述|
|:-:|:-:|
|\_\_STDC\_\_|是否符合 `ANSI C` 标准，符合则为1|
|\_\_STDC_VERSION\_\_|`C` 语言标准版本号，十进制整数常量|

> `__STDC_VERSION__` 与 `C` 标准对应关系

|`C` 标准|值|
|:-:|:-:|
|C90 / ANSI C|无（但 \_\_STDC\_\_ 定义为 1）|
|C99|199901|
|C11|201112|
|C17|201710|
|C23|202311|

#### MSVC宏

这些宏由 `MSVC` 编译器定义，仅在 `MSVC` 编译环境下有效。

1. 编译器版本相关

|宏|描述|
|:-:|:-:|
|_MSC_VER|`MSVC` 编译器的版本号，十进制整数常量|
|_MSC_FULL_VER|`MSVC` 编译器完整版本号，十进制整数常量|
|_MSC_BUILD|`MSVC` 编译器构建号，十进制整数常量|

2. 平台和架构相关

|平台架构|宏|
|:-:|:-:|
|Windows 32bit|_WIN32|
|Windows 64bit|_WIN64 <br>（在Windows 64bit下同样会定义 `_WIN32`）|
|x86|_M_IX86|
|x86_64|_M_X64|
|Arm 32bit|_M_ARM|
|Arm 64bit|_M_ARM64|

3. 编译选项相关

|宏|描述|
|:-:|:-:|
|_DEBUG|是否为调试模式，调试模式下会被定义|
|NDEBUG|是否非调试模式（即发布模式），发布模式下会被定义|
|_CRT_SECURE_NO_WARNINGS|是否禁用安全性检查，禁用安全性检查时会被定义|

> 可以在源代码中手动定义 `_CRT_SECURE_NO_WARNINGS` 来禁用安全性检查。

## 编译器指令

`#pragma` 用于设置编译器相关的功能，包括优化控制、警告抑制、数据对齐、头文件保护等。

一般情况下用处较少。

## 写在最后

预处理器是程序编译的第一步，是代码不可缺少的一部分。
