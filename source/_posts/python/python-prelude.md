---
title: Python-序
description: Python从入门...
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-24 13:25 +0800
nav:
  next: 
    path: posts/python/python-environment
    title: Python-环境配置
--- 

# 前言

本教程基于`Python3`，主要使用官方`CPython`解释器，以及`Anaconda`虚拟环境。

此外，若没有特殊提及，所有符号均为半角英文符号（ASCII符号）。


<Details>
<Summary>符号对照</Summary>

|符号|中文名称|ASCII编码|注 |
|:--:|:------:|:-------:|:-:|
|!|感叹号|0x21|
|"|双引号|0x22|
|#|井号|0x23|Number sign|
|$|美元符|0x24|
|%|百分号|0x25|Mod|
|&|和|0x26|And|
|'|单引号|0x27|
|\(|左括号|0x28|
|\)|右括号|0x29|
|*|星号|0x2a|
|+|加号|0x2b|
|,|逗号|0x2c|
|-|减号|0x2d|
|.|点|0x2e|
|/|正斜线|0x2f|正斜杠|
|:|冒号|0x3a|
|;|分号|0x3b|
|<|小于号|0x3c|
|=|等于号|0x3d|
|>|大于号|0x3e|
|?|问号|0x3f|
|@|艾特|0x40|At|
|\[|左方括号|0x5b|中括号|
|<span>\\</span>|反斜线|0x5c|反斜杠|
|\]|右方括号|0x5d|中括号|
|^|插入符|0x5e|
|_|下划线|0x5f|
|`|重音符|0x60|
|\{|左花括号|0x7b|大括号|
|\||竖线|0x7c|
|\}|右花括号|0x7d|大括号|
|~|波浪号|0x7e|

</Details>

<Details>
<Summary>系统环境</Summary>

- OS: Windows 10 Pro for Workstation, 10.0.19045.4894(Win10 22H2 2022 Update), 64bit, English / Archlinux x86_64, Linux 6.12.10-arch1-1

- Processor: Intel Core i9-13900HX@5.2GHz

- DRAM: DDR5 5600MHz, 16Gx2

- Python-Version: 3.11.3

- Conda-Version: 24.3.0

- Anaconda-Version: 1.12.3

</Details>

# 为什么选择Python

`Python`是高级语言，语法灵活且十分接近自然语言，对新手很友好，上手难度低，适合入门。~~（当然也可能是大学程设课要学）~~。

`Python`有极强的扩展性，常被成为胶水语言，可以包装用其他语言写的库并通过`Python`的方式更高效的调用（比如Numpy、Torch），也可以去实现各种千奇百怪的轮子。想用``Python`写些什么时，不妨先找找有没有符合要求的轮子，以免重复造轮子。

`Python`语法灵活意味着条条框框少，但是代价是什么？对于一门编程语言来说，条条框框越少，在后面等着你的深渊就越大。`Pythno`可不是一条小蛇（Python英文原意是巨蟒）。

# 关于Python

`Python`是解释型语言，依赖解释器（Interpreter）执行。`Python`的源代码一般储存在`.py`拓展名的文件中，在运行时由解释器逐行解释执行，所有操作都由解释器完成。`Python`是开源的，官方发行版的解释器由C语言实现，除此之外，还有由`Java`实现的`IronPython`、`Python`实现的`PyPy`等。

解释型语言的优点是开发效率高，debug方便快捷，但缺点是运行效率低下，不过`Python`并不在乎。卷运行效率并不是`Python`的赛道，`Python`的主业还是超级胶水，靠强大的扩展性和开发效率打出自己的天地。

与C语言家族不同，`Python`不是静态类型的语言，变量的类型是运行时可变的。因此，`Python`没有强制类型检查。虽然可以用一些标记注明变量类型，但是解释器会忽略这些注明，即使货不对板。这些标记更多的是辅助包的使用者或者帮助IDE进行类型检查。事实上，随着近年来IDE的发展，基本可以实现`Python`在开发时的静态类型校验。

```python
#Python

a:int = "Python"
# a被标注为int类型，但是依然可以为a赋字符串类型的值
def func(param:float,lst:list) -> int:
    """
    函数的参数部分和返回值也可以有类似的注明，方便调用
    """
    return 114514
"""切记，这些类型标注会被解释器忽略，如果非要传货不对板的参数，解释器依然会原样执行，然后Boom。下面是"""

imstr:str = 114514
imstr.find("1")
# 报错
"""
Traceback (most recent call last):
    imstr.find("1")
AttributeError: 'int' object has no attribute 'find'
"""
```

但是，`Python`与传统意义上的弱类型语言有所不同（比如`JavaScript`）。`Python`不允许不能隐式转换的类型之间的相互操作（比如int类型和str类型相加），`JavaScript`则允许。

```javascript
//JavaScript

114 + '514'
//> '114514'
```

```python
#Python

114 + '514'
#报错了
"""
Traceback (most recent call last):
TypeError: unsupported operand type(s) for +: 'int' and 'str'
"""
```

# Python的语法结构

`Python`的解释器会从第一行开始逐行解释执行，这样出现的比较早的语句是访问不到出现较晚的语句的。

```python
#python

greet()

def greet():
    print("Hello")
#报错
#NameError: name 'greet' is not defined
```

在`Python`源代码的第一行可以指定解释器和文件编码，而且对这两者的指定必须在第一行完成。
```python
#!/usr/bin/env python3
```

`Python`文件编码默认为`UTF-8`，不建议更改这个，如果编码不对建议手动改回。

`Python`可以分出代码块，作为函数、循环、判断、类的主体。`Python`的代码块以缩进区分，缩进相同的代码共属于一个代码块。如果缩进不对，代码块区分是会乱套的。~~（这也是为什么能看到说不好好写缩进就发配去写Python）~~

# 轮子

`Python`有规模庞大的拓展库/包（即轮子），官方名称是模块，能满足方方面面乱七八糟的需求。__切记不要重复造轮子__，除非你觉得自己造的轮子比别人的轮子好用。

`Python`的包管理器是`pip`，用于安装，升级，卸载这些大大小小的模块

# 命令行

既然是解释型语言，由编译器逐行解释执行，那能不能手动一行行执行呢？当然是可以的。可以通过解释器执行源码文件，也可以执行用户输入的命令。

```bash
python
Python 3.11.7 | packaged by Anaconda, Inc. | (main, Dec 15 2023, 18:05:47) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> print("hello")
hello
>>> 114 + 514
628 
>>> 
```
在命令提示符节目输入即可进入Python命令行，在这里可以交互式直接执行`Python`代码。

# 写在最后

~~你已经掌握Python了，快去手搓Transformer吧~~

`Python`很简单，也很复杂。适合入门，但是要学明白却很不容易。这，才刚刚开始。