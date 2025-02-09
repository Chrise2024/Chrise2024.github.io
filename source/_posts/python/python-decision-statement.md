---
title: Python-判断语句
description: 对，对吗？
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-09 19:46 +0800
nav:
  prev:
    path: posts/python/python-collection
    title: Python-集合类型
  next:
    path: posts/python/python-loop-statement
    title: Python-循环语句
--- 

# 判断语句

在程序逻辑中，经常会遇到一些不同的条件并根据条件进行不同的操作，这里便是判断语句的业务范围。

`Python`中判断语句分为两类：`if`语句和`match`（于`Python3.10`加入）语句。

```python
#Python

if condition:
    #需要执行的语句

if condition:
    #需要执行的语句
elif condition:
    #需要执行的语句
else:
    #需要执行的语句

if condition:
    #需要执行的语句
else:
    #需要执行的语句
```

`condition`为条件表达式，决定语句的执行逻辑。`condition`为真则执行`if`后的代码块。`if`语句可以独立存在，也可以在后面附加`elif`进行连续判断，或者在最后加上`else`作为条件未命中的操作。一个`if`语句可以包含任意数量的`elif`以及至多一个`else`且必须出现在最后。

> `if`后接的必须是有缩进的代码块

## match语句

`match`语句用于对条件的多种情况分别进行匹配，基本语法如下：

```python
#Python

match condition:
    case expr1:#可以有不限数量个case
        #需要执行的语句
    case expr2:
        #需要执行的语句
    case _:#可选，非必须
        #blblbl
```

`condition`为条件，`match`会从上到下一次把`condition`和`case`后的表达式比较，如果相同则命中，执行对应`case`下的语句。

`expr`为匹配表达式，可以进行值匹配、模式匹配以及通配。

1. 值匹配

这是最基本的匹配，检测`condition`是否与`expr`相等。`expr`只能是常量或常量表达式。

```python
#Python

value = 443

match value:
    case 22:
        print("ssh")
    case 80:
        print("http")
    case 443:
        print("https")
"""
https
"""
```

2. 模式匹配

这是一种测试目标是否具有某种特定特征的匹配模式，可以进行更精确的匹配。

```python
#Python

tuple1 = (1,2)
tuple2 = (1,1)
tuple3 = (2,1)

def tuple_match(tpl):
    match tpl:#匹配元组的元素
        case (x,y) if x == y:
            print("Equal")
        case (x,y) if x > y:
            print("Greater")
        case (x,y) if x < y:
            print("Less")

tuple_match(tuple1)
tuple_match(tuple2)
tuple_match(tuple3)
"""
Less
Equal
Greater
"""

complex_number1 = 2+4j
complex_number2 = 1+3j

def complex_match(cml):
    match cml:#匹配对象的属性
        case complex(real = 2):
            print("Real = 2")
        case complex(imag = 3):
            print("Imag = 3")

complex_match(complex_number1)
complex_match(complex_number2)
"""
Real = 2
Imag = 3
"""

arr1 = [1,2,3,4]
arr2 = [1,7,3,4]

def array_match(arr):
    match arr:#匹配列表元素
        case [1,2,3,4]:#精确匹配列表，实际上是值匹配
            print("[1,2,3,4]")
        case [1,_,3,4]:#模糊匹配
            print("[1,?,3,4]")

array_match(arr1)
array_match(arr2)
"""
[1,2,3,4]
[1,?,3,4]
"""
```

3. 通配

通配用于匹配所有情况，一般作为`match`语句的兜底（类似`C`的`default`）。

```python
#Python

tuple1 = (1,2)

def tuple_match(tpl):
    match tpl:#匹配元组的元素
        case (x,y) if x == y:
            print("Equal")
        case (x,y) if x > y:
            print("Greater")
        case _:
            print("Less")
"""
Less
"""
```

## if...else 语句

这是一种特殊的表达式，根据条件执行不同的操作，产生不同的值。

```python
#Python

#expr1 if condition else expr2

homo = 114514
res = 114 if homo > 10086 else 514
print(res)
homo = 1234
res = 114 if homo > 10086 else 514
print(res)
"""
114
514
"""
```

在`condition`为真时，会执行`expr1`并将其值作为整个表达式的值，`expr2`被忽略；`condition`为假时，会执行`expr2`并将其值作为整个表达式的值，`expr1`被忽略。

# 写在最后

判断语句允许根据不同的条件执行不同的代码，极大提高了程序的灵活性。