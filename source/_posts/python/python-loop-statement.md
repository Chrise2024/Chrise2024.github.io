---
title: Python-集合类型
description: 我能装万物
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-05 19:31 +0800
nav:
  prev:
    path: posts/python/python-decision-statement
    title: Python-判断语句
  next:
    path: posts/python/python-function-and-decorator
    title: Python-函数与装饰器
--- 

有时在程序中，需要将某一部分逻辑重复许多次（比如轮询、计数、监听），这时候就需要循环语句出场了。

`Python`中循环语句有两类：`for`、`while`。

## while循环

`while`循环通常用于循环次数未知的循环或无限循环。

### while循环基本用法

```python
#Python

while condition:
    #需要执行的语句

while condition:
    #需要执行的语句
else:
    #在循环结束后执行的语句
```

`condition`为循环进行的条件，为真时会执行循环体，为假时则结束循环。在循环开始前会进行一次判断。

> `condition`只能使用单条语句。

> `while`后是具有缩进的代码块，称为循环体。

```python
#Python

i = 0
j = 10

while i < 5 and j > 6:
    print("i=%d,j=%d" %(i,j))
    i += 1
    j -= 2

while True:
    #死循环

```

### while循环控制语句

在`while`循环内部，可以使用一些特殊的循环控制语句：`break`、`continue`

1. `break`，用于结束当前所在的循环。如果存在多层循环嵌套则只会结束当前层而不会影响外层。

2. `continue`，用于立即结束当前轮循环并开始新一轮循环。

```python
#Python

i = 0

while i < 20:
    i += 1
    if i % 2 == 0:
        continue
    print(i)
    if i > 12:
        break
"""
1
3
5
7
9
11
13
"""
```

## for循环

`for`循环用于遍历集合中的元素（即迭代，见 [迭代器、生成器](../python-iterator-and-generator)），可以配合内置`range`函数实现有限次数的循环。

> **`range`函数**<br>`range(start, stop[, step = 1]) -> range / range(stop) -> range`，返回一个包含指定区间内的数的可迭代序列。<br>
> `start`：区间起始位置，在使用`range(stop)`时为0<br>`stop`：区间终止位置<br>`step`：每个数之间的差<br>即：$\left \{x|x = \text{start} + k \cdot \text{step},\text{start} \leq x < \text{stop},k \in \mathbb{N} \right \}$

### for循环用法

```python
#Python

for loop_var in iterable:
    #需要执行的语句
```

`iterable`为可迭代类型或可迭代类型表达式（元组、字符串、列表、字典、集合均为可迭代类型）。

> `loop_var`为迭代变量，是可迭代类型中元素的拷贝。

> `for`后是具有缩进的代码块，称为循环体。

> 在对字典使用`for`时，只会遍历字典的键。

```python
#Python

lst = [1,3,5,7,9]

for i in lst:
    print(i,end=' ')

print()

for j in range(5):
    print(j,end=' ')
"""
1 3 5 7 9 
0 1 2 3 4 
"""
```

### for循环控制语句

在`for`循环内部，可以使用一些特殊的循环控制语句：`break`、`continue`，用法及作用同`while`。

```python
#Python

for i in range(20):
    if i % 2 == 0:
        continue
    print(i)
    if i > 12:
        break
"""
1
3
5
7
9
11
13
"""
```

## 写在最后

循环语句极大简化了程序中的重复逻辑，但是要注意循环控制的逻辑，避免死循环或无效循环。
