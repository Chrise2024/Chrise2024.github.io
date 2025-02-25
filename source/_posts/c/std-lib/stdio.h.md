---
title: C标准库-stdio.h
description: stdio.h
tags: [Programing,C,C-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-24 21:19 +0800
---

`stdio.h` 提供文件读写以及标准IO操作。

## 类型

1. `size_t`，为 `sizeof` 的返回值，是 `unsigned long long` 的别名。

2. `FILE` ，文件流类型，一般使用其指针，称为文件流。

> 在从任何流中读写数据或向流写入数据时，会先将数据存入缓冲区，然后在特定条件下将数据从缓冲区输出到流或者从缓冲区读取数据。缓冲区由编译器自动分配，大小为 `BUFSIZ` Byte。

> 对流进行读取时，读取多少数据，流的位置就会向后移动多少。

3. `fpos_t`，文件位置类型。

## 宏

1. `NULL`，表示空指针，值为 `0LL`。

2. `_IOFBF`、`_IOLBF` 和 `_IONBF`，适用于 `setvbuf` 函数的第三个参数。

3. `BUFSIZ`，十进制整数，表示 `setbuf` 函数使用的缓冲区大小。

4. `EOF`，表示文件末尾，值为-1。

5. `FOPEN_MAX`，十进制整数，系统可以同时打开文件的最大数量。

6. `FILENAME_MAX`，十进制整数，系统允许的最大文件名长度。

7. `L_tmpnam`，十进制整数，`tmpnam` 函数创建的临时文件名的最大长度。

8. `SEEK_CUR`、`SEEK_END` 和 `SEEK_SET`，适用于 `fseek` 函数定位文件不同位置。

9. `TMP_MAX`，十进制整数，`tmpnam` 函数可生成的独特文件名的最大数量。

10. `stderr`、`stdin` 和 `stdout`， `FILE*` 类型，表示标准错误流、标准输入流、标准输出流。

## 函数

### 文件与流

1. `void clearerr(FILE* stream)`

清除 `stream` 的文件结束和错误标识符。

2. `int fclose(FILE* stream)`

关闭文件流 `stream` 并刷新所有缓冲区。

3. `int feof(FILE* stream)`

检测 `stream` 是否已经到达末尾，即流当前位置是否为文件结束标识符，默认为 `EOF`。若达到末尾返回非0值，否则返回0。

```c
#include <stdio.h>
#include <errno.h>

int main(){
    FILE* fp = fopen(__FILE__,"r");
    char c;
    
    if(fp == NULL)
   {
        printf("%s\n",strerror(errno));
        return -1;
    }
    
    while (!feof(fp)){
        c = fgetc(fp);
        putchar(c);
    }
    fclose(fp);
    return 0;
}
//这段代码会输出自身内容
```

4. `int ferror(FILE* stream)`

测试流是否设置了错误标识。是则返回非0值，否则返回0。

5. `int fflush(FILE* stream)`

刷新流 `stream`。刷新成功返回0，否则返回 `EOF` 并给流设置错误标识。

6. `int fgetpos(FILE* stream, fpos_t* pos)`

获取 `stream` 当前位置并储存进 `pos` 指向的内存区域中

```c
FILE* fp;
fpos_t position;

fp = fopen(__FILE__,"w+");
fgetpos(fp, &position);
```

7. `FILE* fopen(const char* filename, const char* mode)`

用于打开文件并创建文件流。

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

8. `size_t fread(void* ptr, size_t size, size_t nmemb, FILE* stream)`

用于从 `stream` 读取 `size * nmemb` 字节的数据并储存进 `ptr` 指向的内存区域。

```c
#include <stdio.h>
#include <errno.h>

int main(){
    FILE* fp = fopen(__FILE__,"r");
    char buffer[64] = {0};
    
    if(fp == NULL)
   {
        printf("%s\n",strerror(errno));
        return -1;
    }
    
    fread(buffer,24,sizeof(char),fp);
    printf("%s\n",buffer);
    fclose(fp);
    return 0;
}
```

9. `FILE* freopen(const char* filename, const char* mode, FILE* stream)`

关闭原有文件并打开一个新文件。

```c
//等效于
fclose(stream);
stream = fopen(const char* filename, const char* mode);
```

10. `int fseek(FILE* stream, long int offset, int whence)`

修改文件流的位置到相对 `whence` 偏移为 `offset` 的位置。

```c
#include <stdio.h>
#include <errno.h>

int main(){
    FILE* fp = fopen(__FILE__,"r");
    char buffer[64] = {0};
    
    if(fp == NULL)
   {
        printf("%s\n",strerror(errno));
        return -1;
    }
    
    fseek(fp,16,SEEK_SET);
    fread(buffer,24,sizeof(char),fp);
    printf("%s\n",buffer);
    fclose(fp);
    return 0;
}
```

> 宏 `SEEK_CUR`、`SEEK_END` 和 `SEEK_SET` 分别表示文件流的开始位置、结束位置和当前位置。 `offset` 可以为负，表示向前移动。

11. `int fsetpos(FILE* stream, const fpos_t *pos)`

将文件流的位置移动到 `pos` 对应的位置。

```c
#include <stdio.h>

int main(){
    FILE* fp;
    fpos_t position;

    fp = fopen("file.txt","w+");
    fgetpos(fp, &position);
    fputs("Hello, World!", fp);

    fsetpos(fp, &position);
    fputs("Overrided Content", fp);
    fclose(fp);

    return 0;
}
```

12. `long int ftell(FILE* stream)`

返回文件流 `stream` 当前位置。

13. `size_t fwrite(const void* ptr, size_t size, size_t nmemb, FILE* stream)`

将 `ptr` 指向的内存区域的前 `size * nmemb` 个字节写入到文件流 `stream`。

```c
#include <stdio.h>

int main(){
    FILE* fp;

    fp = fopen("file.txt","w+");
    fgetpos(fp, &position);
    fwrite("Hello, World!",8,1, fp);
    fclose(fp);

    return 0;
}
```

14. `int remove(const char* filename)`

删除文件。

15. `int rename(const char* old_filename, const char* new_filename)`

将文件 `old_filename` 重命名为 `new_filename`。

16. `void rewind(FILE* stream)`

将文件流 `stream` 的位置移动到开头。

17. `void setbuf(FILE* stream, char* buffer)`

指定 `stream` 使用的缓冲区。

```c
char* buffer = (char*)malloc(BUFSIZ);
FILE* fp = fopen(__FILE__,"r");
setbuf(fp,buffer);
```

18. `int setvbuf(FILE* stream, char* buffer, int mode, size_t size)`

设置文件流 `stream` 的缓冲模式为 `mode`。`buffer` 为指派的缓冲区， `size` 为缓冲区大小。

`mode` 分为以下三种模式，分别由三个宏定义：

|宏|模式|描述|
|:-:|:-:|:-:|
|_IOFBF|全缓冲模式|向流写入时在缓冲区被填满时刷新，<br>从流读取时只有在有读取请求且缓冲区为空时将数据填入缓冲区|
|_IOLBF|行缓冲模式|向流写入时在缓冲区被填满时或写入换行符时刷新，<br>从流读取时只有在有读取请求且缓冲区为空时将数据填入缓冲区，直到遇到换行符|
|_IONBF|无缓冲模式|不使用缓冲区，所有对流的IO操作都会实时刷新|

19. `FILE* tmpfile(void)`

以 `wb+` 模式创建文件，文件将在流被关闭或程序运行结束后被删除。

20. `char* tmpnam(char* str)`

以 `wb+` 模式创建文件，返回文件名并将文件名储存进 `str`，文件将在流被关闭或程序运行结束后被删除。

### printf系

1. `int printf(const char* format, ...)`

用于格式化输出。`format` 称为格式化字符串，格式化字符串中的格式化占位符会被替换为对应格式的表达式值。这些占位符都以半角百分号 `%` 开头，每种占位符对应不同的数据类型。

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

2. `int sprintf(char* str, const char* format, ...)`

`printf` 的字符串输出的版本，相当于将 `printf` 原本要输出到 `stdout` 的数据储存到 `str` 中。

```c
char* buffer = (char*)malloc(BUFSIZ);
sprintf(buffer,"This %s is %d years old.\n","dog",4);
printf("%s\n",buffer);
```

3. `int fprintf(FILE* stream, const char* format, ...)`

`printf` 的文件流输出版本，相当于将 `printf` 原本要输出到 `stdout` 的数据输出到到文件流 `stream` 中。

```c
FILE* fp;
fp = fopen("file.txt","w");
fprintf(fp,"This %s is %d years old.\n","dog",4);
```

4. `int snprintf ( char*  str, size_t size, const char*  format, ... );`

`sprintf` 的改版，规定了输出的最大长度 `size`，输出长度超过 `size` 的部分会被截断。

```c
char* buffer = (char*)malloc(BUFSIZ);
sprintf(buffer,"This %s is %d years old.\n","dog",4);
printf("%s\n",buffer);
```

5. `int vprintf(const char* format, va_list arg)`

`printf` 的参数列表版本，相当于把原本在 `...` 中的参数压缩进参数列表 `arg` 中。

6. `int vsprintf(char* str, const char* format, va_list arg)`

`sprintf` 的参数列表版本，相当于把原本在 `...` 中的参数压缩进参数列表 `arg` 中。

7. `int vfprintf(FILE* stream, const char* format, va_list arg)`

`fprintf` 的参数列表版本，相当于把原本在 `...` 中的参数压缩进参数列表 `arg` 中。

### scanf系

1. `int scanf(const char* format, ...)`

用于读取输入。在调用 `scanf` 函数时，程序会阻塞，直到可以从缓冲区读取数据。

```c
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

2. `int sscanf(const char* str, const char* format, ...)`

`scanf` 的字符串读取版本，相当于 `scanf` 从 `str` 中读取数据而不是 `stdin` 。

```c
int a;
char c[64];
char buffer[64];
gets(buffer);
sscanf("%d %s",&a,c);
```

3. `int fscanf(FILE* stream, const char* format, ...)`

`scanf` 的文件流读取版本，相当于 `scanf` 从文件流中读取数据而不是 `stdin` 。

```c
FILE* fp;
fp = fopen(__FILE__,"r");
int a;
char c[64];
fscanf(fp,"%d %s",&a,c);
```

### put系

1. `int puts(const char* str)`

等效于 `printf("%s\n",str)`。

2. `int putchar(int char)`

等效于 `printf("%c\n",char)`，返回 `char`。

3. `int fputs(const char* str, FILE* stream)`

等效于 `fprintf(stream,"%s\n",str)`。

4. `int fputc(int char, FILE* stream)`

等效于 `fprintf(stream,"%c\n",char)`，返回 `char`。

5. `int putc(int char, FILE* stream)`

等效于 `fputc`，但是会将流的位置向前移动。

### get系

1. `char* gets(char* str)`

相当于 `scanf("%s",str)`，若读取成功则返回 `str`，读取失败则返回 `NULL` 。

> `gets` 读取字符串时，会顺带移除缓冲区中字符串后的空字符。

> 如果缓冲区中第一个字符是空字符，`gets` 会读取到空字符串。

2. `int getchar(void)`

从 `stdin` 中读取一个字符并返回这个字符。

3. `char* fgets(char* str, int n, FILE* stream)`

`gets` 的文件流读取版本，相当于 `gets` 从文件流中读取数据而不是 `stdin` 。

4. `int fgetc(FILE* stream)`

`getchar` 的文件流读取版本，相当于 `getchar` 从文件流中读取数据而不是 `stdin` 。

5. `int getc(FILE* stream)`

从文件流 `stream` 中读取一个字符并返回这个字符，但是会将流的位置向前移动。

6. `int ungetc(int char, FILE* stream)`

将字符插入文件流 `stream`，使下一个被读取的字符是这个字符。

### 杂项

1. `void perror(const char* str)`

将有关程序当前错误的描述性信息输出到 `stderr`。等效于 `fprintf(stderr,"%s: %s\n",str,strerror(errno))`
