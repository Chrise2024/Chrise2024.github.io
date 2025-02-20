---
title: Python-入门第一课
description: Hello world, Python
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-26 17:03 +0800
nav:
  prev:
    path: posts/python/python-environment
    title: Python-环境配置
  next:
    path: posts/python/python-variable
    title: Python-变量
--- 

欢迎来到 `Python` 大世界。 `Python` 的语法很灵活且符合直觉，上手简单，但是基本语法还是值得一讲的。

## 基本单位

1. 语句。 `Python` 的基本单位为语句，一行为一条语句。如果语句很长，可以用反斜杠\分割语句到多行，即多行语句。 [], {}, 或 ()中的多行语句则不需要反斜杠。

```python
#Python

a = 1 + 1 + \
    1 * 5 / \
    1 % 4
l = [1,2
     3,4]
```

2. 标识符。标识符是程序中变量、函数、数组、类等的名字，由字母（大写或小写）、数字和下划线组成，但第一个字符必须是字母或下划线。此外， `Python` 对大小写敏感， `Author` 和 `author` 就是不同的标识符。

> 在 `Python` 中，有一些名称被定义为关键字，供程序逻辑使用，不能作为标识符名称。一下列出 `Python` 中的关键字。

|||||||
|:-:|:-:|:-:|:-:|:-:|
| False| None| True| and| as|
| assert| async| await| break| class|
| continue| def| del| elif| else|
| except| finally| for| from| global|
| if| import| in| is| lambda|
| nonlocal| not| or| pass| raise|
| return| try| while| with| yield|

3. 代码块。 `Python` 用缩进来区分代码块，连续的（或之间有更高阶缩进）具有相同缩进的行为一个代码块。

```python
#Python

def func():
  '''这是一个代码块'''
  lololo
  ababa
  ccccc

if __name__ == '__main__':
  '''这也是一个代码块'''
  xcxcxcx
  def greet():
    pass
  lxkjsfjnsd
```

4. 函数。函数是语句的集合，可以执行特定功能并返回值。详见 [函数与装饰器](../python-function-and-decorator) 章节。

5. 控制结构，包含逻辑控制与循环控制。详见 [判断语句](../python-decision-statement)、[循环语句](../python-loop-statement) 章节。

6. 类，类是 `Python` 面向对象编程的核心机制，是用来描述具有相同的属性和方法的对象的集合。详见 [类与对象](../class-and-object) 章节。

7. 注释。 `Python` 中有两种注释，单行注释和多行注释。单行注释以井号#（Hash）开头，占据从注释开始到当前行结束的所有区域；多行注释在首尾以""""""或''''''包裹（本质上是个字符串），可以占据多行。

```python
#这是单行注释
a = 6;#这也是单行注释

'''
这
是
多
'''
"""
行
注
释
"""
```

<Details>
<Summary>关于注释</Summary>
Python的注释在编译时会被解释器器完全忽略，不会影响代码逻辑，也不是强制性的。注释是写给人看的，用于注明代码功能，解释代码逻辑等一系列增加代码可读性的操作，还有一个用处是临时关闭一些不需要的语句（直接给某个语句注释调就可以在不删除语句的情况下跳过执行这条语句）。

写好注释是个好习惯，不仅为了自己，也是为了别人。
</Details>

## 基本输入输出

 `Python` 提供了内置的输入输出函数 `input()` 和 `print()` 用于执行命令行IO操作。

### input()

 `input([prompt:any]) -> str` ，返回输入的字符串。可选参数  `prompt` ，用于在Console上显示输入时的提示语句

```python
#Python

a = input("Input a Number") #a为str
```

### print

 `print(*objects [, sep=' '][, end='\n'][, file=sys.stdout][, flush=False]) -> None` ，无返回值。

- `*objects`  要输出的值，可以传入很多变量给 `print` 进行输出，函数会把这些变量输出在窗体上，两个变量之间默认用空格分隔。 `print` 会把所有常规传入的参数视为要输出的用心，对输出的设置则需要使用关键字参数。

- `sep`  指定输出之间的分隔符。

- `end`  输出完所有内容后在行尾输出的内容。

- `file`  写入的文件对象，默认值为 `sys.stdout` ，即窗体输出。

- `flush`  是否强制刷新，能否设置成功取决于写入对象。

```python
#Python

print(1,2,3,4)          #输出多个变量
print("abababa",end='') #输出完不换行
print(1,2,3,4,sep='=')  #指定分隔符
```

## 好习惯

### 标识符命名

 `Python` 中大部分内容由程序员定义，各种标识符随着程序体量的增大而错综复杂起来，此时良好的命名规范就发挥作用了。有意义且详细的标识符名称有助于在大堆代码中厘清逻辑，提高代码的可读性。标识符在命名时宁长勿短（除了循环控制变量或用处只有紧挨着几行的临时变量），同一个程序中遵循同一种命名规则。

### 注释

注释也是十分重要的一部分。不光别人可能会看你的代码，注释可以帮助别人快速了解代码；而且你自己过段时间回来，你也不一定记得请这段代码是干嘛的，这时候注释就可以帮助你回忆。

### 导入模块

包是 `Python` 中十分重要的一部分，使用 `import` 或者 `from ... import ...` 语句导入。在导入包时，应注意一行只导入一个包且杜绝 `form ... import *` 。

### 代码中的空格与空行

 `Python` 中空格一般用于分割标识符。其余地方的空格会被忽略。但是任然建议在一些地方加入空格以提高代码可读性（例如运算符两侧）

```python
#Python
a = 1 + 1 * 4 / 5 ** 1 % 4
a=1+1*4/5**1%4
#第一种看起来会更舒服更明确
```

空行对解释器来说没有实际意义，但是可以用于分隔不同功能的代码区段，有利于增加可读性。

## 写在最后

这里只是 `Python` 的基本理论，也是学习的第一课。 `Python` 水很深，越是简单的语言，在暗处收的“税”就越多，前往不要轻敌哦。
