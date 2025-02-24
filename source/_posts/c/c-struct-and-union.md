---
title: C-结构体与共用体
description: 很能装
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-20 17:12 +0800
nav:
  prev:
    path: posts/c/c-string
    title: C-字符串
  next:
    path: posts/c/c-io-operation
    title: C-IO操作
---

结构体、共用体以及从结构体衍生出来的位域是 `C` 中用于存储不同类型数据的类型，可由用户定义。三种类型中储存的数据称为成员，可以是任意数据类型。

## 结构体

结构体 `struct` 是三种类型中最基本、最常用的。

### 定义结构体

```c
struct tag {
    members_1;
    members_2;
};

struct Date{
    int year;
    int month;
    int day;
};

struct {
    char name[64];
    int age;
    double gpa;
} Student;
```

- `tag` 为结构体标签，在声明结构体类型的变量时会用到。

- `members` 为结构体成员，行事和声明变量一样，成员之间用分号分隔。

> `tag` 与成员所在的代码块位置可以互换

### 结构体的使用

在定义结构体后，就可以使用结构体标签声明结构体类型的变量了。

```c
#include <stdio.h>
#include <string.h>

struct Student{
    char name[64];
    int age;
    double gpa;
};

int main() {
    struct Student stu;
    stu.age = 24;
    stu.gpa = 1.14514;
    strcpy(stu.name,"Homo");
}
```

> 结构体在定义时可以附带声明一个或多个结构体类型的变量

```c
struct Student{
    char name[64];
    int age;
    double gpa;
} stu;
//只有这种类型的结构体声明可以用于声明变量
```

> **结构体类型 VS 结构体变量**<br>结构体类型相当于一张图纸，在定义结构体后，图纸上的内容就确定了。但是图纸不能直接使用，于是需要使用这张图纸去声明变量，用这张图纸声明的所有变量都具有这张图纸规定的所有属性（结构体成员）。

> 结构体变量在声明时需要使用 `struct tag variable` ，很麻烦也不美观，此时可以使用 `typedef` 为结构体起个别名

```c
struct Student{
    char name[64];
    int age;
    double gpa;
};
typedef struct Student Student;//别名可以与结构体标签相同
Student stu;//用别名声明变量

typedef struct {
    char name[64];
    int age;
    double gpa;
} Student;//也可以在结构体声明时就定义
Student stu;
```

### 结构体的占用大小

结构体变量的每个成员都需要占用内存空间，所有成员占用的总空间即结构体的大小。看上去结构体的大小即所有成员占用大小之和，但是实际情况并非如此。

在程序编译与运行过程中，会进行对齐（即数据对齐， `Align` ）。将数据按自然边界对齐（比如 `int` 对齐在4Byte而不是插在5Byte中间），能够提高数据一致性，提高访存性能（未对齐的数据会导致 `Arm` 架构的处理器工作异常，导致程序崩溃）。

对于结构体而言，内部数据一般都是整Byte，为了数据一致性提高性能，编译器会将每个成员占用空间对齐到相同大小，如果成员本身没有占用这么大则填充空字节（比如对齐长度为8Byte时，会给4Byte的 `int` 填充4Byte的空字节在末尾）。

结构体在进行对齐时，会以结构体内除了数组和结构体类型的成员中占用空间最大的类型为基准，将其他所有成员（除大小大于这一基准的数组、结构体）对齐到这一大小。此外，如果声明顺序连续的几个变量占用空间之和小于或等于基准，则会将这几个变量视为一个整体进行对齐。

```c
struct Student{
    char name[64];//64x1Byte 数组
    int age;//4Byte int
    double gpa;//8Byte double
};//这个结构体会以8Byte的double为基准，将所有成员对齐到这一大小，不包括64Byte（大于8Byte）的大数组

struct Student1{
    char name[5];//5x1Byte 数组
    int age;//4Byte int
    double gpa;//8Byte double
};//这个结构体会以8Byte的double为基准，将所有成员对齐到这一大小，包括5Byte（小于8Byte）的小数组

struct Seat {
    struct Student owner;//80Byte 结构体
    double posx;//8Byte double
    double posy;//8Byte double
    char number;//1Byte char
    int size;//4Byte int | 此处number和size总大小为5Byte小于8Byte，会将二者视为一个整体进行对齐
};//这个结构体会以8Byte的double为基准，将所有成员对齐到这一大小，不包括80Byte（大于8Byte）的大结构体

int main() {
    printf("%d\n",sizeof(struct Student));
    printf("%d\n",sizeof(struct Student1));
    printf("%d\n",sizeof(struct Seat));
}
// 80
// 24
// 104
```

### 结构体变量的初始化

结构体变量在声明后编译器会为变量分配内存空间，规则同一般变量，大小为结构体的大小。和数组一样，结构体声明后同样会被垃圾数据占据，此时需要对其进行初始化。结构体初始化可以使用和数组一样的初始化其，也可以手动初始化。

```c
#include <stdio.h>
#include <string.h>

struct Student{
    char name[64];
    int age;
    double gpa;
};

int main() {
    struct Student stu1 = {
        "Bob",
        18,
        3.9
    };//初始化器会按照结构体成员声明的先后顺序将数据填入结构体成员
    struct Student stu2;
    strcpy(stu2.name,"Alice");
    stu2.age = 17;
    stu2.gpa = 4.3;
}
```

### 访问结构体成员

访问结构体成员使用成员访问运算符 `.` ： `struct.member` 。

```c
struct Student{
    char name[64];
    int age;
    double gpa;
} stu;

stu.age = 114;
printf("%d\n",stu.age);
```

### 结构体指针

结构体作为数据类型自然存在指向结构体的指针，即： `struct tag* ptr` ，和普通指针基本一致。

> 与普通指针不同的是，结构体指针允许直接通过指针访问结构体的成员，而不是先使用 `*` 访问结构体本身再访问成员。使用结构体指针访问成员使用访问运算符 `->` 。

```c
#include <stdio.h>
#include <string.h>

struct Student{
    char name[64];
    int age;
    double gpa;
} stu;

int main() {
    struct Student* ptr = &stu;
    ptr->age = 114;
    printf("%d\n",ptr->age);
}
```

## 结构体作为函数参数

可以向函数传入结构体，函数声明与定义如下。

```c
#include <stdio.h>
#include <string.h>

struct Student{
    char name[64];
    int age;
    double gpa;
};

void print_info(struct Student stu);
void print_info(struct Student);

void print_info(struct Student stu){
    printf("Student %s is aged %d wiht GPA %lf\n", stu.name, stu.age, stu.gpa);
}

int main() {
    struct Student stu = {
        "Alice",
        17,
        4.3
    };
    print_info(stu);
}
// Student Alice is aged 17 wiht GPA 4.300000
```

在向函数传入结构体是，和一般变量一样，传入的是数据的拷贝。对形参结构体变量的修改不会影响到实参。

### 从函数返回结构体

可以从函数返回结构体，函数声明与定义如下。其余细节和函数返回变量一致，此处不再赘述。

```c
struct Student generate_student_info(char name[],int age,double gpa);

struct Student generate_student_info(char name[],int age,double gpa){
    struct Student stu;
    strcpy(stu.name,name);
    stu.age = age;
    stu.gpa = gpa;
    return stu;
}
```

## 位域

位域 `bit field` 是结构体的衍生，与结构体不同的是，位域可以指定成员占用的位（`bit`,1 `Byte` = 8 `bit`）数。位域的成员会被限制在指定位数中上下限会受其影响，除此之外与普通结构体完全一致。

### 定义位域

```c
struct tag {
    members_1 : size;
    members_2 : size;
};

struct Flags{
    unsigned char a : 1;
    unsigned char b : 1;
    unsigned char c : 1;
    unsigned int d : 5;
}
```

位域定义与结构体定义基本一致，只是在成员声明后加上了 `: size` 以指定占用位数。

## 共用体

共用体 `union` 类似于结构体，但是内部所有成员共用内存空间，成员之间空间不独立，同时只能有一个成员有效。

### 定义共用体

```c
union tag {
    members_1;
    members_2;
};

union DataSet{
    char id[20];
    int status_code;
    double hight;
    char flag;
};

union DataSet ds;
```

共用体定义与结构体定义一致，此处不再赘述。

### 共用体成员

共用体中所有成员共用内存，因此在同一时间只有一个成员有效，其余成员数据为无效数据。虽然可以访问，但是其值会是各种奇怪的垃圾值。

共用体的占用空间等于成员中除数组和其他结构体、共位体外成员占用空间的最大值的整数倍。

```c
#include <stdio.h>
#include <string.h>

union DataSet{
    char id[20];
    int status_code;
    double hight;
    char flag;
};

void print_union(union DataSet ds){
    printf("========================\n");
    printf("id:         %s\n",ds.id);
    printf("status_code:%d\n",ds.status_code);
    printf("hight:      %lf\n",ds.hight);
    printf("flag:       %c\n",ds.flag);
    printf("========================\n");
}

int main() {
    printf("Union Size: %d\n",sizeof(union DataSet));
    union DataSet ds;
    strcpy(ds.id,"Hello,world");
    print_union(ds);
    ds.status_code = 404;
    print_union(ds);
    ds.hight = 114.514;
    print_union(ds);
    ds.flag = '*';
    print_union(ds);
    
}
/*
Union Size: 24
========================
id:         Hello,world
status_code:1819043144
hight:      8783543638392452200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000.000000
flag:       H
========================
========================
id:         ?
status_code:404
hight:      8783541188878974300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000.000000
flag:       ?
========================
========================
id:         7堿`鍫\@rld
status_code:1614907703
hight:      114.514000
flag:       7
========================
========================
id:         *堿`鍫\@rld
status_code:1614907690
hight:      114.514000
flag:       *
========================
*/
```

可以看到，在对其中一个成员赋值时，其他成员的完整性被破坏了。

## 写在最后

结构体、位域、共用体作为数据的集合，能够极大提高数据传输、处理的效率，并配合函数指针实现“伪类”的效果。
