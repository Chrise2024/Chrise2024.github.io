---
title: C标准库-string.h
description: string.h
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-18 19:44 +0800
---

 `string.h` 是 `C` 的标准库之一，提供与字符串操作相关的函数。

## 宏

1. `NULL`  表示空指针，本质上为 `0LL` 。

## 类型

1. `size_t`  为 `sizeof` 的返回值，是 `unsigned long long` 的别名。

## 函数

1. `void *memchr(const void *str, int c, size_t n)`

返回 `str` 所指向的字符串的前 `n` 个字字节中第一次出现字符 `c` 的位置。

> `c` 为字符的 `ASCII` 编码，返回一个无类型的指针指向该位置，未找到则返回 `NULL` 。

> 尽管 `c` 为 `int` 类型，但是实际上只会用到 `c` 的低8位，即 `c % 256` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "http://www.shigure.link";
    char ch = '.';
    char *res;
    res = (char*)memchr(str, ch, strlen(str));
    printf("|%s|\n",res);
}
// |.shigure.link|
```

2. `int memcmp(const void *str1, const void *str2, size_t n)`

比较 `str1` 和 `str2` 指向的内存空间的前 `n` 个字节。

> `str1`  > `str2` 时，返回结果大于0<br> `str1`  =  `str2` 时，返回结果等于0<br> `str1`  <  `str2` 时，返回结果小于0<br>注： `ANSI C` 只规定了返回值的正负性，具体返回值及其特殊意义在不同编译器中各不相同。

> `str1` 和 `str2` 不止局限于字符串，其他数组也可以。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[] = "abcde";
    printf("%d\n", memcmp(str1,str2,sizeof(str1)));
}
// -1
```

3. `void *memcpy(void *dest, const void *src, size_t n)`

将 `src` 指向的储存区的前n个字节复制到 `dest` 指向的储存区，返回 `dest` 。

> `dest` 和 `src` 不止局限于字符串，其他数组也可以。

> 如果 `src` 和 `dest` 区域有重叠，则会导致数据被破坏或者其他未定义行为。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[6];
    memcpy(str2,str1,sizeof(str1));
    printf("%s\n", str2);
}
// ABCDE
```

4. `void *memmove(void *dest, const void *src, size_t n)`

同 `memcpy` ，但是在 `src` 和 `dest` 区域有重叠时会更加安全，不过速度会慢于 `memcpy` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[6];
    memmove(str2,str1,sizeof(str1));
    printf("%s\n", str2);
}
// ABCDE
```

5. `void *memset(void *str, int c, size_t n)`

将指定的值 `c` 复制到 `str` 所指储存区的前n个字节，返回 `str` 。

> 函数不会做边界检查，谨防缓冲区溢出。

> 尽管 `c` 为 `int` 类型，但是实际上只会用到 `c` 的低8位，即 `c % 256` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[16] = "ABCDE";
    memset(str1,'6',sizeof(str1));
    printf("%s\n", str1);
}
// 666666
```

6. `char *strcat(char *dest, const char *src)`

把 `src` 所指向的字符串追加到 `dest` 所指向的字符串的末尾，返回 `dest` 。

> 函数不会进行边界检查，谨防缓冲区溢出。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[16] = "ABCDE";
    char str2[] = "FGHIJ";
    strcat(str1,str2);
    printf("%s\n",str1);
}
// ABCDEFGHIJ
```

7. `char *strncat(char *dest, const char *src, size_t n)`

把 `src` 所指向的字符串的前n个字节追加到 `dest` 所指向的字符串的末尾，返回 `dest` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[16] = "ABCDE";
    char str2[] = "FGHIJ";
    strncat(str1,str2,2);
    printf("%s\n",str1);
}
// ABCDEFG
```

8. `char *strchr(const char *str, int c)`

返回字符 `c` 在字符串 `str` 中第一次出现的位置，未找到则返回 `NULL` 。

> 尽管 `c` 为 `int` 类型，但是实际上只会用到 `c` 的低8位，即 `c % 256` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "http://www.shigure.link";
    char ch = '.';
    char *res;
    res = (char*)strchr(str, ch);
    printf("|%s|\n", res);
}
// |.shigure.link|
```

9. `int strcmp(const char *str1, const char *str2)`

比较字符串 `str1` 和 `str2` （字典序）。

> `str1`  > `str2` 时，返回结果大于0<br> `str1`  =  `str2` 时，返回结果等于0<br> `str1`  <  `str2` 时，返回结果小于0<br>注： `ANSI C` 只规定了返回值的正负性，具体返回值及其特殊意义在不同编译器中各不相同。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[] = "abcde";
    printf("%d\n", strcmp(str1,str2));
}
// -1
```

10. `int strncmp(const char *str1, const char *str2, size_t n)`

比较字符串 `str1` 和 `str2` 的前 `n` 个字节。

> `str1`  > `str2` 时，返回结果大于0<br> `str1`  =  `str2` 时，返回结果等于0<br> `str1`  <  `str2` 时，返回结果小于0<br>注： `ANSI C` 只规定了返回值的正负性，具体返回值及其特殊意义在不同编译器中各不相同。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[] = "abcde";
    printf("%d\n", strncmp(str1,str2,4));
}
// -1
```

11. `int strcoll(const char *str1, const char *str2)`

同 `strcmp` ，但是会基于当前系统语言规则进行比较（本地化）。

```c
//C

#include <stdio.h>
#include <string.h>
#include <locale.h>

int main() {
    setlocale(LC_COLLATE, "en_US.UTF-8");

    char str1[] = "Apple";
    char str2[] = "Banana";
    printf("%d\n", strcoll(str1,str2));
}
// -1
```

12. `char *strcpy(char *dest, const char *src)`

将字符串 `src` 复制到字符串 `dest` ，返回 `dest` 。

> 函数不会进行边界检查，谨防缓冲区溢出。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[6];
    strcpy(str2,str1);
    printf("%s\n", str2);
}
// ABCDE
```

13. `char *strncpy(char *dest, const char *src, size_t n)`

将字符串 `src` 的前n个字节复制到字符串 `dest` ，返回 `dest` 。

> 函数不会进行边界检查，谨防缓冲区溢出。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[] = "HIJKL";
    strncpy(str2,str1,2);
    printf("%s\n", str2);
}
// ABJKL
```

14. `size_t strcspn(const char *str1, const char *str2)`

返回 `str1` 从开头连续有几个字符不包含于 `str2` 中。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[] = "DERFGT";
    printf("%d\n", strcspn(str1,str2));
}
// 3
```

15. `char *strerror(int errnum)`

返回错误代码对应的描述错误的字符串，未找到则返回 `NULL` 。

```c
//C

#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE *file = fopen("non_existent_file.txt", "r");
    if (file == NULL) {
        printf("Error: %s\n", strerror(errno));
    }
    return 0;
}
// No such file or directory
```

16. `size_t strlen(const char *str)`

返回字符串的长度（不包括结束符）。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    printf("%d\n", sizeof(str1));
    printf("%d\n", strlen(str1));
}
// 6
// 5
```

17. `char *strpbrk(const char *str1, const char *str2)`

返回 `str1` 中第一个出现在 `str2` 中的字符的位置，未找到则返回 `NULL` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "ABCDE";
    char str2[] = "DERFGT";
    printf("%c\n", *strpbrk(str1,str2));
}
// D
```

18. `char *strrchr(const char *str, int c)`

返回字符 `c` 在字符串 `str` 中最后一次出现的位置，未找到则返回 `NULL` 。

> 尽管 `c` 为 `int` 类型，但是实际上只会用到 `c` 的低8位，即 `c % 256` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "http://www.shigure.link";
    char ch = '.';
    char *res;
    res = (char*)strrchr(str, ch);
    printf("|%s|\n", res);
}
// |.link|
```

19. `size_t strspn(const char *str1, const char *str2)`

返回 `str1` 中第一个不存在于 `str2` 中的字符的索引，未找到则返回结束符的索引。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "RTFABCDE";
    char str2[] = "DERFGT";
    printf("%d\n", strspn(str1,str2));
}
// 3
```

20. `char *strstr(const char *str1, const char *str2)`

返回字符串 `str2` 在 `str1` 中第一次出现的位置，未找到则返回 `NULL` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "Hello,world";
    char str2[] = "llo";
    printf("%s\n", strstr(str1,str2));
}
// llo,world
```

21. `char *strtok(char *str, const char *delim)`

分割 `str` ， `delim` 为分割的分隔符，返回被分割的第一个字符串。

> `str` 为 `NULL` 时，返回下一个分割出的字符串下一个字符串不存在时则返回 `NULL` 。

```c
//C

#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "Hello world ,welcome to C";
    char *token = strtok(str, " ");
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, " ");
    }
    return 0;
}
/*
Hello
world
,welcome
to
C
*/
```

22. `size_t strxfrm(char *dest, const char *src, size_t n)`

根据系统当前区域转换字符串 `src` 的前n个字符并储存在 `dest` 中，返回被转换的字符串长度，不包括结束符。可用于 `strcmp` 比较，组合使用时，等效于 `strcoll` 。

```c
//C

#include <stdio.h>
#include <string.h>
#include <locale.h>

int main() {
    setlocale(LC_COLLATE, "en_US.UTF-8");

    char str1[] = "apple";
    char str2[] = "banana";

    char transformed1[100], transformed2[100];

    strxfrm(transformed1, str1, 100);
    strxfrm(transformed2, str2, 100);

    printf("%d\n",strcmp(transformed1, transformed2));
}
// -1
```
