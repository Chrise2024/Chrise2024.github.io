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
