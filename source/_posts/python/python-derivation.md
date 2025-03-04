---
title: Python-推导式
description: 我寻思
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-03-04 21:11 +0800
nav:
  prev:
    path: posts/python/python-function-and-decorator
    title: Python-函数与装饰器
  next:
    path: posts/python/python-iterator-and-generator
    title: Python-迭代器与生成器
--- 

在 [集合类型](../python-collection) 章节，我们知道可以使用 `[]` 、 `{}` 或 `()` 表达式来创建集合类型或元组。除了通过明文的方式，将初始元素写在表达式内，还可以使用推导式，将另一个集合类型内的元素映射到当前集合。

## 列表类推导式

最基本的推导式，用法如下：

```python
expr for x in iterable#expr表达式即映射关系，用于转换迭代变量 `x`。
expr for x in iterable if condition#condition为条件，如果条件为假，生成器会忽略这一元素。
2*x for x in range(10)
x for x in range(10) if x%2 ==0
```

推导式会生成一个生成器对象，包含所有源集合的元素的映射。

> 推导式本质上会生成一个生成器对象，能够实现创建集合实际上是一个语法糖，当其位于集合类型中时会被解释器展开。

```python
[x for x in range(5)]#生成器对象包括 0 1 2 3 4几个元素
[0,1,2,3,4]#会被解释器展开成这样
```

这样的推导式可以用于 `set`、`list`、`tuple` 等列表性质的类型。

```python
{x for x in range(5)}#set
[x for x in range(5)]#list
tuple(x for x in range(5))#tuple
```

> 由于元组初始化的性质，单一的生成器对象会被之间识别为变量，所以需要使用 `tuple` 函数来创建元组。

## 字典推导式

和列表类推导式类似，只是 `expr` 必须为键值对表达式。

```python
{key_expr:value_expr for x in iterable}
{key_expr:value_expr for x in iterable if condition}

{str(x):x*2 for x in range(5)}
{str(x):x**2 for x in range(10) if x % 2 == 0}
"""
{'0': 0, '1': 2, '2': 4, '3': 6, '4': 8}
{'0': 0, '2': 4, '4': 16, '6': 36, '8': 64}
"""
```

推导式可以用于配合 `range` 函数快速生成集合类型，也可以将现有集合的元素映射到新集合，十分有用。
