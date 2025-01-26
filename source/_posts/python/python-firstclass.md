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

# 基础语法

欢迎来到`Python`大世界。`Python`的语法很灵活且符合直觉，上手简单，但是基本语法还是值得一讲的。

## 基本单位

1. 语句。`Python`的基本单位为语句，一行为一条语句。如果语句很长，可以用反斜杠\分割语句到多行，即多行语句。 [], {}, 或 ()中的多行语句则不需要反斜杠。 

```python
#Python

a = 1 + 1 + \
    1 * 5 / \
    1 % 4
l = [1,2
     3,4]
```

2. 标识符。标识符是程序中变量、函数、数组、类等的名字，由字母（大写或小写）、数字和下划线组成，但第一个字符必须是字母或下划线。此外，`Python`对大小写敏感，`Author`和`author`就是不同的标识符。

3. 代码块。`Python`用缩进来区分代码块，连续的（或之间有更高阶缩进）具有相同缩进的行为一个代码块。

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

4. 函数。函数是语句的集合，可以执行特定功能并返回值。详见 [函数](../python-function) 章节。

5. 控制结构，包含逻辑控制与循环控制。详见 [判断语句](../python-decision-statement)、[循环语句](../python-loop-statement) 章节。

6. 类，类是`Python`面向对象编程的核心机制，是用来描述具有相同的属性和方法的对象的集合。详见 [类与对象](../class-and-object) 章节。

7. 注释。`Python`中有两种注释，单行注释和多行注释。单行注释以井号#（Hash）开头，占据从注释开始到当前行结束的所有区域；多行注释在首尾以""""""或''''''包裹（本质上是个字符串），可以占据多行。

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

# 好习惯

`Python`中大部分内容由程序员定义，各种标识符随着程序体量的增大而错综复杂起来，此时良好的命名规范就发挥作用了。有意义且详细的标识符名称有助于在大堆代码中厘清逻辑，提高代码的可读性。标识符在命名时宁长勿短（除了循环控制变量或用处只有紧挨着几行的临时变量）。

此外，注释也是十分重要的一部分。不光别人可能会看你的代码，注释可以帮助别人快速了解代码；而且你自己过段时间回来，你也不一定记得请这段代码是干嘛的，这时候注释就可以帮助你回忆。

# 写在最后

这里只是`Python`的基本理论，也是学习的第一课。`Python`水很深，越是简单的语言，在暗处收的“税”就越多，前往不要轻敌哦。

下一节 [变量与常量](../python-variable-and-const)