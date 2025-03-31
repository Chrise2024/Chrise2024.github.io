---
title: 位元旗标
description: 更好的储存数据的方式
tags: [Programing]
categories: Programing
math: true
date: 2025-03-31 20:47 +0800
lang: zh-CN
--- 

在程序中，有一些类型会包含许多表示二元的属性，或者函数会包含这样的参数，这些属性或参数被称为旗标。旗标的最大特点是只有两种状态，可以用布尔类型表示。

```cpp
class Service{
public:
    bool IsActive;
    bool IsBackground;
    bool AutoStart;
    void setStatus(bool isActive,bool isBackground,bool autoStart){
        ...
    }
}
```

直接简单粗暴的给每个旗标开一个属性或者安排一个参数固然可行，但是旗标数量一多，一大片地方都是旗标，显然不太优雅。那么有什么解决方法吗？有的，这就是今天的主角：位元旗标。

旗标一般用布尔类型表示，只需要一个位就能储存。再看其他类型，比如 `int` ，有32个位，那么能否将32个旗标塞进这个 `int` 呢？可行，位元旗标就这样诞生了。位元旗标顾名思义，就是用变量的不同位来储存多个旗标。

将一个 `int` 以二进制形式写出来，大概长这样：

```cpp
0b1000100010000010111101010011111
```

那么如何读取这个 `int` 中储存的32个旗标呢？旗标只有两种属性，读取某个旗标只用关心那一位是0还是1就可以了。为了获取那一位的值，可以使用只在那一位是1、其余位都是0的 `int` 来和这个 `int` 位与一下。

```cpp
int flag = 0b1000100010000010111101010011111;
int test = 0b0000000000000000010000000000000;
int result = flag & test;
```

在这个例子中，用 `test` 和 `flag` 位与，`test` 除了目标位以外的位均为0，与 `flag` 对应位位与的结果只会是0，这样结果就仅由两个数目标位位与的结果决定。如果 `flag` 对应位为0，位与结果就为0；对应位为1，结果就会是 `test` 自身，实际应用中只用判断是否大于0即可。

位元旗标可以通过位与读取，也可以组装。与读取对应，组装使用位与。

```cpp
#define SEG_1 0b00000001
#define SEG_2 0b00000010
#define SEG_3 0b00000100
#define SEG_4 0b00001000
#define SEG_5 0b00010000
#define SEG_6 0b00100000

int flag = SEG_1 | SEG_4 | SEG_6;
```

这个例子中，这些宏通过位或组合，位或保留位信息，使得 `flag` 变量中对应位的值为1，这样就组装出来了一个位元旗标。

这样一个位元旗标可以承载多个旗标，读取时使用位与读取，极大提高了存储密度。

```cpp
#define SEG_1 0b00000001
#define SEG_2 0b00000010
#define SEG_3 0b00000100
#define SEG_4 0b00001000
#define SEG_5 0b00010000
#define SEG_6 0b00100000

int flag = SEG_1 | SEG_4 | SEG_6;
printf("%d\n",(flag & SEG_1 ) > 0);
printf("%d\n",(flag & SEG_2 ) > 0);
printf("%d\n",(flag & SEG_3 ) > 0);
printf("%d\n",(flag & SEG_4 ) > 0);
printf("%d\n",(flag & SEG_5 ) > 0);
printf("%d\n",(flag & SEG_6 ) > 0);
/*
1
0
0
1
0
1
*/
```
