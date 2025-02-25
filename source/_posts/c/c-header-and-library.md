---
title: C-头文件与库
description: 井include 《studio。h》
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-24 14:45 +0800
nav:
  prev:
    path: posts/c/c-preprocessor
    title: C-预处理器
  next:
    path: posts/c/c-error-handle
    title: C-错误处理
---

头文件是拓展名为 `.h` 的文本文件，内容为标准 `C` 代码。根据习惯，头文件储存函数声明、宏定义等内容。

头文件大体分为两类：标准库的头文件和自定义的头文件。标准库头文件使用 `#include <file>` 引入，自定义头文件使用 `#includ "file"` 引入。

> 任何文件都可以像头文件一样通过 `#include` 预处理器指令引入，但是习惯上使用 `.h` 文件。

> 头文件中一般只有函数声明而没有函数定义。函数会被定义在其他源文件或二进制库文件中，在编译时由编译器链接。当然，执意要在头文件内定义函数也是可以的。

`ANSI C` 标准提供了许多标准库，分别对应不同的头文件，提供了许多用于IO、数学计算、内存管理的函数和宏。

这些标准库直接与操作系统和目标平台对接，几乎已经成为了 `C` 语言本身的一部分。

|标准库头文件|描述|
|:-:|:-:|
|assert.h|提供宏 `assert`，用于断言检查|
|complex.h|提供复数运算的函数和宏|
|ctype.h|提供字符处理函数|
|errno.h|提供错误码变量 `errno`，用于处理错误|
|fenv.h|提供对浮点环境的控制|
|float.h|定义浮点类型的限制值|
|limits.h|定义各种类型的限制值|
|locale.h|提供与地域化相关的函数和宏|
|math.h|提供科学计算函数|
|setjmp.h|提供非本地跳转功能的宏和函数|
|signal.h|提供处理信号的函数和宏|
|stdarg.h|提供处理可变参数函数的宏|
|stdbool.h|提供布尔类型的宏|
|stddef.h|定义了一些通用类型和宏|
|stdint.h|定义了精确宽度的整数类型|
|[stdio.h](../std-lib/stdio.h)|提供标准输入输|
|stdlib.h|提供标准库函数|
|[string.h](../std-lib/string.h)|提供字符串操作函数|
|tgmath.h|提供泛型数学函数|
|time.h|提供时间和日期函数|

## 写在最后

如果需要写一段代码实现某些功能，不妨取标准库里找找有没有现成的函数，避免重复造轮子。
