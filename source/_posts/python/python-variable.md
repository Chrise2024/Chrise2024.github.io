---
title: Python-变量
description: 变量，变量
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-01-27 12:54 +0800
nav:
  prev:
    path: posts/python/python-firstclass
    title: Python-入门第一课
  next:
    path: posts/python/python-operator-and-number
    title: Python-运算符与数字
--- 

# 前言

`Python`中的变量不用声明，在直接对变量赋值后变量才会被创建。变量本质上指向内存中的数据，访问变量时，就相当于访问对应内存空间的内容。

> 通过内置的`id()`函数，可以得到变量在内存中的地址。

`Python`变量的赋值直接使用`=`，等号左侧为变量名，右侧为变量值。也可以为多个变量同时赋值，也可以为多个变量赋同一个值。

如果不想使用某个变量了，可以用`del`语句删除变量，释放对应的内存空间。

```python
#Python

a = 1
b = 2
a,b,c = 1,[],"666" #为多个变量赋不同值
a=b=c=1234   #为多个变量赋同一个值

del a
del b,c
```

`Python`是动态类型语言，每个变量的数据随时可变，因此不存在类型检查，也不存在“类型”这一概念。通常说的“类型”只是变量所指的内存中对象的数据类型。尽管可以通过一些标注注明变量类型，但是解释器会忽略这些注明。这些注明就像注释一样，辅助阅读代码。

在程序中，可以使用内置的`type()`函数来查看变量对应的数据类型。

```python
#Python

#变量的类型可以自由变换
var1 = 111    #int
var2 = "1234" #str
var3 = 1.1    #float

a:str = "Python"
b:list = [1,1,4,5,1,4]
def func(param:float,lst:list) -> int:
    """
    函数的参数部分和返回值也可以有类似的注明，方便调用
    参数和返回值都可注明
    """
    return 114514

print(type(var1))
print(type(var2))
print(type(var3))
print(type(a))
print(type(b))
print(type(func))
"""
<class 'int'>
<class 'str'>
<class 'float'>
<class 'str'>
<class 'list'>
<class 'function'>
"""
```

# 基本数据类型

`Python`中常见的数据类型如下

1. Number（数字），包括

- 整数 `int`
- 浮点（小数） `float`
- 布尔 `bool`（`bool`是逻辑真假，但是在与数字进行操作时，`True`会被转换为1，`False`会被转换为0。事实上，`bool`类型是`int`的子类）
- 复数 `complex`（复数类型的实部与虚部均为`float`类型）

详见 [运算符与数字](../python-operator-and-number) 章节。

```python
#Python

int_var,float_var,bool_var,complex_var = 10086,114.514,False,3+4j
complex_var = complex(114,514)#也可以创建复数

#Python还支持通过不同进制创建变量，通过数字前缀标识
dec = 1234          # 默认十进制
binary = 0b0100101  # 0b前缀为为二进制
octo = 0o1234567    # 0o前缀为八进制
hexa = 0x1A2B3E     # 0x前缀为十六进制
sci  = 6.22E-34     # 科学记数法，但是不强制要求真-科学记数法格式
#并且数字之间支持插入不限数量的下划线_（每两个数字间最多插入1个下划线）来分割数字位。
big_number = 1_0000_0000       # 这是1亿
million = 1_000_000            # 这是1M
mac_addr = 0x1E_29_FF_64_D5_B3 # 分割Mac地址
strange_number = 1_2_3_4_5_6_7 # What f*** is it? But it is valid.
```

2. String（字符串），使用单引号或双引号括起来，引号的数量不限，由三个引号括起来的字符串可以换行。在字符串内部可以使用反斜杠插入换行符。

详见 [元组与字符串]((../python-tuple-and-string)) 章节。

```python
#Python

string = "asdfg" # 这是一个字符串
string = 'aaaaa' # 这也是一个字符串
multi_line_string = """
这是一个多行字符串
可以跨几行
"""
multi_line_string = '''
这是一个多行字符串
可以跨几行
用单引号也可以
'''
escape_string = "Hello\nWorld" #带转义符的字符串，\n表示换行符
```

3. Tuple（元组）。元组是多个数据的不可变集合，可以包含多个不同类型的变量。元组写在一对括号内，元素之间用逗号隔开。元组内的数据顺序排列，可以通过索引访问元组中的元素，但是不能修改。



```python
#Python

tuple_var = (1234,1.1,"主",True,[3],{})
print(tuple_var[0])#获取元组的第一个元素

#尝试修改元组的元素
tuple_var[0] = 1
#报错
"""
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
"""
```

4. List（列表）。与元组类似，列表也是数据集合，但是与元组的区别在于列表是可变的。列表写在一对方括号内，元素之间用逗号隔开。列表内的数据顺序排列，可以通过索引访问列表组中的元素，但是不能修改。

详见 [集合类型]((../python-collection)) 章节。

```python
#Python

list_var = [1234,1.1,"主",True,[3],{}]
print(list_var[0])#获取列表的第一个元素

#尝试修改列表的元素
list_var[0] = 1
print(list_var)
"""
[1, 1.1, '主', True, [3], {}]
"""
```

5. Set（集合）。集合同样是数据集合的一种，但是它的名字就和数学上的集合一样，内部元素不能重复。集合写在一对花括号内，元素之间用逗号隔开。与列表不同，集合中的元素是无序的。集合可以像在数学中一样取交集、并集等操作。

> 如果用{}来创建空集，默认创建的是字典而不是集合。创建空集合必须使用`set()`。

详见 [集合类型]((../python-collection)) 章节。

```python
#Python

set_var = {"C++","Go","Rust","Python","Java","Js"}

set_another = {"Go","Python","Ts","Ruby"}
print("Go" in set_var)
"""
True
"""
print(set_var & set_another) #交集
"""
{'Go', 'Python'}
"""
print(set_var | set_another) #并集
"""
{'Rust', 'Ts', 'Js', 'Python', 'Java', 'C++', 'Ruby', 'Go'}
"""
print(set_var - set_another) #差集
"""
{'C++', 'Rust', 'Js', 'Java'}
"""
print(set_var ^ set_another) #不同时存在
"""
{'Rust', 'Ts', 'Js', 'Java', 'C++', 'Ruby'}
"""
```

6. Dictionary（字典）。字典是无序的集合，通过键值对映射（Key-Value-Map）来存取值，在同一个字典中，键必须是唯一的。字典由一对大括号包裹，内部元素为`Key`:`Value`的集合。

> 如果用{}来创建空集，默认创建的是字典而不是集合。

详见 [集合类型](../python-collection) 章节。

```python
#Python

dict_var = {"Fortran":1957,"C":1972,"Python":1991}
print(dict_var["C"])#获取字典的值

#修改字典
dict_var["Python"] = "Ohhhhhh"
print(dict_var)
"""
{'Fortran': 1957, 'C': 1972, 'Python': 'Ohhhhhh'}
"""
```

> 可变类型与不可变类型<br>在这些数据类型中，`Number`，`String`，`Tuple`类型是不可变的。虽然可以为这些类型的变量重新赋值，但是实际上是创建了一个新值并丢弃原有的旧值。<br>`List`，`Set`，`Dict`类型是可变的，内部元素可以修改，不会对变量重新赋值。
```python
#Python

a = 123
print(id(a))
"""
140714327667304
"""
a = 456 #创建了新整数456并赋值给a，原有的123被丢弃
print(id(a)) #前后a的变量地址不同，说明不是同一个变量
"""
2262400349040
"""

b = [1,2,3,4]
print(id(b))
"""
2262403777920
"""
b[1] = 10086
print(id(b)) #前后b的变量地址相同，说明是同一个变量
"""
2262403777920
"""
```

> 在上述结论中，不可变类型`Number`，`String`，`Tuple`被称为值类型，储存的是变量本身，当变量的值发生改变时，实际上是创建了一个新的对象，并将变量指向这个新的对象；可变类型`List`，`Set`，`Dict`被称为引用类型，储存的是变量的引用，当变量的值发生改变时，原始对象的值也会被改变。<br>这两种类型的差异除了体现在赋值上，在 [函数](../python-function) 的参数传递上也会体现。

</Details>

# 数据类型转换

很多时候，我们需要对数据类型进行转换。`Python`中的类型转换分为两种：

- 隐式类型转换

- 显示类型转换

## 隐式类型转换

隐式类型转换通常在程序运行过程中自动发生，一般出现在不同类型数据之间进行逻辑、算数或比较运算时，不需要手动操作。`Python`通常会将较低的类型转换为较高的类型以避免数据丢失（低精度数据转化为高精度数据，派生级别较高的类转换为派生级别较低的类）。

<Details>
<Summary>数据类型的高低之分</Summary>

“较高”数据类型指的是能够表示更多信息（或更精确信息）的数据类型，而“较低”的数据类型则表示的信息较少。

比如：complex > float > int

复数可以表示浮点，自己还有虚部；浮点可以表示整数，也能表示其他所有小数。

对于类而言，为了避免数据丢失，会将派生等级较高的类转换为派生等级较低的类，因为子类中可能会包含父类没有的成员或方法，都转换为子类会丢失数据。

比如 int > bool

bool派生自int，在相互运算时会将bool转为int。

</Details>


```python
#Python

a = 1234 #int
b = 114.514 #float
print(type(a + b)) #浮点的精度高于整数，因此会将int转为float再进行计算。
"""
<class 'float'>
"""
#结果为float型

c = True #bool
d = 0 #int
print(type(c + d)) #bool为int的子类，会将bool转换为父类int再进行计算
"""
<class 'int'>
"""
#结果为int型

#不能隐式转换的情况
s = "主"
i = 6
s + i
#报错
"""
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
"""
#说明不能隐式转换
```

## 显式类型转换

有些情况下，数据类型并不能隐式转换，这时候就需要显示转换登场了。显示类型转换通过内置函数手动进行，可以将变量转换为需要的类型，同时可以打破隐式类型转换的规律。

```python
#Python

a = 123
b = 54.6

print(type(a+b))
print(type(a+int(b))) #将b转为int类型再运算
"""
<class 'float'>
<class 'int'>
"""
```

内置类型转换函数

1. `int(x:any [,base:int]) -> int` 将x转换为整数类型

> 可选参数：`base`，指定转换时使用的进制，默认为十进制

2. `float(x:any) -> float` 将x转换为浮点类型

3. `complex(real:number [,imag:number]) -> complex` 创建一个复数

> 可选参数：`imag`，指定虚部，默认为0

4. `bool(x:any) -> bool` 将x转换为布尔

> 任意不为0的数字或不为空的集合/字符串将被转换为`True`，反之则为`False`

5. `str(x:any) -> str` 将x转换为字符串

6. `tuple(s:iterable) -> tuple` 将s转换为元组，不提供s则创建空元组

7. `list(s:iterable) -> list` 将s转换为列表，不提供s则创建空列表

8. `set(s:iterable) -> set` 将s转换为集合，不提供s则创建空集合

9. `dict(s:iterable) -> dict` 将s转换为字典，不提供s则创建空字典

10. `frozenset(s:iterable) -> any` 将s转换为不可变集合

11. `chr(x:int) -> str` 将整数x转换为对应编码的字符

12. `ord(x:str) -> int` 将字符x转换为对应的编码

13. `hex(x:int) -> str` 将整数x转换为十六进制字符串

14. `oct(x:int) -> str` 将整数x转换为八进制字符串

15. `bin(x:int) -> str` 将整数x转换为二进制字符串

16. `eval(x:str) -> any` 将字符串解析为`Python`表达式

```python
#Python

a = eval("4*4")   #16
a = eval("16.27") #16.27
```

> 这些转换并不是只要给一个变量就一定能转换成功。比如`int("1234")`可以转换成功，因为字符串`"1234"`包含了有效的数字信息；`int("Python")`不能转换，因为字符串`"Python"`不包含有效数字信息。在转换不能进行时，会抛出`ValueError`异常。通常情况下，显示类型转换能否完成取决于提供给转换函数的变量是否有足够的信息来表示目标类型。

# 左值与右值

`Python`中有两类表达式：左值与右值。

- 左值：指向内存位置的表达式被称为`左值（lvalue）表达式`。左值可以出现在赋值号的左边或右边。左值主要包括变量与指针。

- 右值：存储在内存中某些地址的数值被称为`右值（rvalue）`。右值是不能对其进行赋值的表达式，可以出现在赋值号的右边，但不能出现在赋值号（等号）的左边。

变量都是左值，可以出现在赋值号左边，如变量声明并初始化。常量为右值，只能出现在赋值号右侧。出现在赋值号左侧的右值会导致编译器报错。

```python
#C

value = 10086#有效
114 = 514#非法语句，报错
```

# 写在最后

无数变量组成了`Python`程序。`Python`中的一切都可以是变量，上至函数、类，下至各种基本数据类型。函数在定义后可以赋值给另一个变量（其类型就是函数的签名），也可以作为参数传递。

```python
#Python

def func():
    pass

myfunc = func

myfunc()
```