---
title: Python-元组与字符串
description: 我们是不可变的
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-03 16:46 +0800
nav:
  prev:
    path: posts/python/python-operator-and-number
    title: Python-运算符与数字
  next:
    path: posts/python/python-collection
    title: Python-集合类型
--- 

# 元组

元组是多个数据的不可变集合，可以包含多个不同类型的变量，每个数据称为元素。

元组写在一对括号内，元素之间用逗号隔开（不用括号也可以）。元组内的数据顺序排列，其相对于第一个元素的位置值称为索引，可以通过索引访问元组中的元素，但是不能修改。

- 可以通过内置的`len()`函数获取元组内元素的数量。

- 可以通过内置的`max()`函数获取元组内最大的元素。

- 可以通过内置的`min()`函数获取元组内最小的元素。

- 可以通过内置的`tuple([iterable])`函数创建空元组或将可迭代类型转为元组。

```python
#Python

tuple1 = (1,"2",[3],{4})
tuple2 = 1,'1',{4},{5:1},[4] #不加括号也可以
empty_tuple = ()
empty_tuple = tuple()
tuple3 = tuple([1,2,3,4])#从列表创建元组
print(len(tuple1))
print(max(tuple3))
print(min(tuple3))
```

> 如果要使用集合表达式创建只有一个元素的元组，需在元素后加速逗号，否则会被识别为普通变量。

```python
#Python

tuple1 = (1)
print(type(tuple1))
tuple2 = (1,)
print(type(tuple2))
"""
<class 'int'>
<class 'tuple'>
"""
```

## 元组元素的访问

元组作为数据的集合，自然是可以访问内部储存的数据的。访问元组中的元素通过访问运算符`[]`，以元素的索引（index）为标志：`tuple[index]`。

> 在计算机中，绝大部分编程语言的集合索引都是从0开始，即下标为0对应第一个元素

> 在`Python`中，索引可以从前到后，也可以从后到前。索引为正数则是从前到后（正向），索引为负数则是从后到前（反向）。0为正向索引的第一个元素，-1为反向索引的第一个元素（即最后一个元素）。


```python
#Python

tuple1 = (1,"2",[3],{4})
print(tuple1[0])#获取第一个元素
print(tuple1[2])#获取第三个元素
print(tuple1[-1])#获取最后一个元素
print(tuple1[-2])#获取倒数第二个元素

"""
1
[3]
[4]
[3]
"""
```

元组作为不可变类型，内部元素不可修改。

```python
#Python

tuple1 = (1,"2",[3],{4})
tuple1[2] = 114514

"""
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
"""
```

## 元组的拆分

元组支持拆分，可以将一个元组同时赋值给与元组内元素相同数目的变量。变量会按先后顺序依次从获取元组对应位置的元素。

```python
#Python

tuple1 = (1,"2",[3],{4})
a,b,c,d = tuple1
print(a,b,c,d)

"""
1 2 [3] {4}
"""
```

## 元组的切片

访问运算符不仅可以用于访问元素，可也有用来获取元组的切片：`tuple[start_index:end_index]`，得到一个新元组，包含原元组从`start_index`到`end_index`的元素，元素顺序不变。

> `Python`中，切片的索引是顾头不顾尾的前闭后开区间，切片会包括从`start_index`对应的元素到`end_index`前一个元素及之间的所有元素，不包括`end_index`对应的元素。

> `start_index`与`end_index`均可为空。`tuple[start_index:]`表示从`start_index`开始的所有元素；`tuple[:end_index]`表示从第一个元素到`end_index`的所有元素（会包括最后一个元素）；`tuple[:]`则表示原元组。

```python
#Python

tuple1 = (1,"2",[3],{4})
print(tuple1[1:3])#实际包含索引为1、2的元素，索引为3的元素未包含
print(tuple1[1:-1])#不包括最后一个元素
print(tuple1[1:])#包括最后一个元素
print(tuple1[:-2])
tuple2 = tuple1[:]
print(id(tuple1) == id(tuple2))
"""
('2', [3])
('2', [3])
('2', [3], {4})
(1, '2')
True
"""
```

## 元组运算符

元组可以像数字一样执行各种运算。

|表达式|结果|描述|
|:-:|:-:|:-:|
|(1, 2, 3) + (4, 5, 6)|(1, 2, 3, 4, 5, 6)|拼接两个元组，得到一个新元组|
|(1, 2, 3)*2|(1, 2, 3, 1, 2, 3)|重复元组并拼接|
|tuple1 = (1, 2, 3)<br>tuple1 += (4, 5, 6)<br>print(tuple1)|(1, 2, 3, 4, 5, 6)|向元组追加元组，得到一个新元组|
|3 in (1, 2, 3)|True|判断值是否存在|
|(1, 2, 3) > (4, 5, 6)|False|比较两个元组|

## 元组的方法

- `tuple.count(value:any) -> any` 返回`value`在元组内出现的次数。

- `tuple.index(value:any [, start:int=0][, stop:int=9223372036854775807]) -> int` 返回`value`在元组内第一次出现位置的索引，不存在则引发`ValueError`异常。

> `start`：寻找的起始位置<br>`stop`：寻找的终止位置

```python
#Python

tuple1 = (1, 2, 3)
print(tuple1.count(2))
print(tuple1.count(4))
print(tuple1.index(2))
print(tuple1.index(4))
"""
1
0
1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: tuple.index(x): x not in tuple
"""
```

# 字符串

字符串是`Python`中最常用的数据类型，是不可变类型。

字符串由成对的单引号或双引号包裹，引号的对数不限。同种引号对数在三对及以上时，字符串可以换行。在一种引号包裹的字符串内部可以使用另一种引号而不会破坏字符传的完整性。

字符串像元组一样，由许多元素组成，字符串的每个字都是一个元素。不过`Python`中没有单字对应的数据类型，每个字依然是字符串。字符串中的元素一般称为字符。

- 可以通过内置的`len()`函数获取字符串内元素（字）的数量。

- 可以通过内置的`max()`函数获取字符串内最大的字符（编码最大）。

- 可以通过内置的`min()`函数获取字符串内最小的字符。

- 可以通过内置的`str([litera])`函数创建空字符串或将其他类型转为字符串。

```python
#Python

str1 = "1234"
str2 = '1234'
str3 = """
multi
line
"""
str4 = '''
multi
line
'''
str5 = str(114514)
str6 = str([1,2,3,4,5])
str7 = ''
str8 = '1"""234"""'
```

## 转义符

在`ASCII`和`unicode`中，有许多特殊字符，无法通过键盘打出。这时候就需要一些特殊的方法“打出”这些字符，转义符就应运而生。

转义符是字符串中以反斜杠开头的特殊字符序列。在反斜杠后加各种字符即可在字符串内被解析为对应的特殊字符。

|转义字符|描述|使用|
|:-:|:-:|:-:|
|\\<br>（在行尾）|续行符|print("11\\<br>45\\<br>14")|
|\\\\ |反斜杠符号|print("\\\\")|
|\\'|单引号|print("\\\'")|
|\\"|双引号|print("\\\"")|
|\\a|响铃|print("\\a")|
|\\b|退格符|print("\\b")|
|\\0|空|print("\\0")|
|\\n|换行符|print("\\n")|
|\\r|回车符|print("\\r")|
|\\t|水平制表符|print("\\t")|
|\\v|垂直制表符|print("\\v")|
|\\f|换页符|print("\\f")|
|\\ooo|八进制数对应的字符（最多转义三位）|print("\\123456")|
|\\xhh|十六进制数对应的字符|print("\\x1E23")|
|\\N{name}|Unicod字符库中对应名称的字符|print("\\N{space}")|
|\\uXXXX|16位十六进制数对应的Unicode字符|print("\\u4E32")|
|\\UXXXXXXXX|32位十六进制数对应的Unicode字符|print("\\UD83EDD13")|

## 字符串前缀

- `u/U` 表示此字符串是`Unicode 字符串`（`Python3`中所有字符串都是unicode字符串，此前缀无实际意义）

- `r/R` 表示此字符串是`原始字符串`,在字符串中禁用转义符

- `b/B` 表示此字符串是`字节字符串`，会被转换为`bytes`

- `f/F` 表示此字符串是`格式化字符串`，可以在字符串中插入`{<expr>}`，其值将被替换为表达式的值。

> `fr`、`br`可以组合使用

```python
#Python

print(u"unicode")
print(r"no\nescape")
print(b"bytes")
print(f"1 + 1 = {1+1}")
"""
unicode
no\nescape
b'bytes'
1 + 1 = 2
"""
```

## 字符串元素的访问

字符串和元组一样，可以访问内部储存的数据。访问字符串中的字符通过访问运算符`[]`，以元素的下标（index）为标志：`tuple[index]`。


```python
#Python

str1 = "114514"
print(str1[0])#获取第一个字符
print(str1[2])#获取第三个字符
print(str1[-1])#获取最后一个字符
print(str1[-2])#获取倒数第二个字符

"""
1
4
4
1
"""
```

字符串作为不可变类型，内部元素不可修改。

```python
#Python

str1 = "114514"
str1[2] = "6"

"""
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
"""
```

## 字符串的切片

和元组一样，字符串也可以切片：`str[start_index:end_index]`，得到一个新字符串，包含原字符串从`start_index`到`end_index`的字符，字符顺序不变。

```python
#Python

str1 = "114514"
print(str1[1:3])#实际包含索引为1、2的字符，索引为3的字符未包含
print(str1[1:-1])#不包括最后一个字符
print(str1[1:])#包括最后一个字符
print(str1[:-2])
str2 = str1[:]
print(id(str1) == id(str2))
"""
14
1451
1145
True
"""
```

## 字符串运算符

字符串可以像数字一样执行各种运算。

|表达式|结果|描述|
|:-:|:-:|:-:|
|"123"+"456"|"123456"|拼接两个字符串，得到一个新字符串|
|"123"*2|"123123"|重复字符串并拼接|
|str1 = "123"<br>str1 += "456"<br>print(str1)|"123456"|向字符串追加字符串，得到一个新字符串|
|'3' in "123"|True|判断字符是否存在|
|"123" > "456"|False|比较两个字符串|

## 格式化字符串

格式化字符串用于格式化输出。格式化字符串中的格式化符号会被替换为对应格式的表达式值。

包含格式化符号的字符串被称为字符串模板，可以通过`%`或`format`方法进行格式化（对应不同的模板类型）。

### %型

这种格式化字符串一脉相承自`C/C++`，格式用法也和`C/C++`完全一致。这种类型的模板使用`%()`进行格式化，规则同`C/C++`的`printf`。


> 模板内如果只有只有一个占位符则 `%()`的括号可以省略

<Details>
<Summary>格式化占位符</Summary>

|符号|描述|
|:-:|:-:|
|%c|格式化字符及其ASCII码|
|%s|格式化字符串|
|%d|格式化整数|
|%u|格式化无符号整型|
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
|\<sp\>|在正数前面显示空格|
|#|在八进制数前面显示零('0')，<br>在十六进制前面显示'0x'或者'0X'<br>(取决于用的是'x'还是'X')|
|0|显示的数字前面填充'0'而不是默认的空格|
|%|'%%'输出一个单一的'%'|
|(var)|映射变量(字典参数)|
|m.n.|m 是显示的最小总宽度,n 是小数点后的位数|

</Details>

<br>

```python
#Python

format_pattern = "This %s is %d years old."
print(format_pattern %("dog",4))
print("This %s is %d years old." %("dog",4))
print("%04d" %12)
print("%.2f" %114.514)
"""
This dog is 4 years old.
"""
```

### {}型

这种字符串模板使用`{}`作为占位符，格式控制写在`{}`内部，占位符格式为`{x:format}`，`x`为占位符顺序，决定了填充占位符的顺序；`format`为格式，使用方式同`%`占位符。`Python`提供了两种模式来格式化这类模板。

- `f-string` 在`Python 3.6`版本加入，通过字符串前缀`f/F`，可以之间在模板占位符中插入表达式来进行格式化（替代占位符顺序）。

- `str.format()` 依靠字符串的`format`方法格式化字符串，类似于`%`插值。

```python
#Python

format_pattern = "This {0:s} is {1:04d} years old."
print(format_pattern.format("dog",4))

print(f"1 + 1 = {1+1}")
"""
This dog is 0004 years old.
1 + 1 = 2
"""
```

## 字符串方法

字符串提供了许多内建的方法。

见 [字符串补充](../python-string-addon)

# 写在最后

元组、字符串与数字是`Python`中使用最多的不可变类型，下一章将开启可变类型的大门。