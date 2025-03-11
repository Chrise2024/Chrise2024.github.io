---
title: C标准库-assert.h
description: assert.h
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-03-11 19:44 +0800
---

## 宏

1. `assert` 用于断言。

断言用于验证对程序运行中对某种情况的假设，并在假设不成立时输出诊断信息并终止程序运行。

```c
assert(expr);
```

`assert` 宏用于检测表达式 `expr` 的真假，当 `expr` 为真（即不等于0）时断言通过，为假时断言失败。

输出的诊断信息包括：

- 触发断言失败的表达式

- 源文件名

- 行号

`assert` 宏只在 `Debug` 环境下有效，在 `Release` 环境下或手动定义宏 `NDEBUG` 时，所有 `assert` 语句都会被编译器忽略。

```c
#include<assert.h>
#include<stdio.h>
#include<stdlib.h>

int main() {
    int a = 1;
    int b = 2;
    assert(a + b == 3);
    assert(a + b != 3);
    printf("End\n");
    return 0;
}
/*
Assertion failed!

Program: xxxx.exe
File: xxxx.c, Line 9

Expression: a + b != 3
*/
```

> 因为在 `Release` 环境下 `assert` 语句会被忽略，一切在语句内对变量的操作都不会执行。
