---
title: Python-函数与装饰器
description: 可以是三角函数
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-03-03 19:46 +0800
nav:
  prev:
    path: posts/python/python-loop-statement
    title: Python-循环语句
  next:
    path: posts/python/python-derivation
    title: Python-推导式
--- 

函数是一组语句的集合，用来执行特定操作，可以有参数决定内部逻辑，也可以再执行完毕后返回一个值。函数可以将复杂的操作封装在一起，避免直接接触内部的业务逻辑，只用关注函数的功能。

## 函数的定义

由于解释器的性质，函数必须定义在使用之前，以保证在调用时函数已经被解释器解析。

定义函数使用 `def` 关键字。

```python
def function_name(parameters):
    #函数体
    #function_name为函数名，标准标识符名称
    #parameters为参数，参数之间用逗号隔开，无参数则留空

def foo(a,b,c,d):
    print(a,b,c,d)
    return

def func(a:int,b:str,c:float,d:list) -> dict:
    #函数体
    #可以像这样给参数和返回值加上类型标记，以供IDE进行类型检查
    #但是解释器不会阻止你传入与标记类型不符的变量
```

> **参数**<br>参数在调用函数时从函数外部传入，供函数内部使用。外部传入的值称为实际参数（实参），函数内部使用的参数为形式参数（形参）。在向函数传递参数时，传入的实际上是参数的拷贝，而不是参数本身。在函数内对形参的修改不会影响实参。

函数在定义完成后可以像一个普通变量一样被使用，或者传递给另一个函数。

```python
def foo():
    pass

func = foo
func()
```

## 调用函数

函数在完成定义后就可以调用了。

```python
def foo(a,b,c,d):
    print(a,b,c,d)
    return

foo(1,123.456,"Homo",[1,2,3,4])
```

## 函数的传参

### 指定参数

在传参时，可以直接给参数名赋值，指定要传入的形参。

```python
def foo3(a,b,c,d):
    print(a,b,c,d)
    return

foo3(1,c=2,d=9,b=7)
"""
1 7 2 9
"""
```

> 指定形参必须出现在调用语句的最后，其后不允许出现普通传参。

```python
def foo3(a,b,c,d):
    print(a,b,c,d)
    return

foo3(1,c=2,d=9,b=7)#正确
foo3(1,c=2,d=9,12)#错误
```

### 在函数内部修改实参

`Python` 中的类型分为可变类型和不可变类型。不可变类型的变量储存的是变量本身；可变类型的变量存储的则是对对象的引用，就像一个人自己和这个人的名片。在传参时，不可变类型的实参会被拷贝一份然后赋值给形参，形参获得的是一个新值；可变类型的实参同样会被复制，但是复制的是引用（就像名片无论怎么复制指向的都是同一个人），对修改的修改会作用到实参上。

```python
def foo(a:int,b:list):
    a += 114#这是不可变类型int
    b.append(514)#这是可变类型list
    return

v1 = 10086
v2 = [1,2,3,4]
foo(v1,v2)
print(v1)
print(v2)
"""
10086
[1, 2, 3, 4, 514]
"""
```

### 参数的默认值

正常情况下，当给函数传入的参数少于参数列表中参数数量时，解释器会报错。由于 `Python` 不允许函数重载，于是提供了参数默认值，可以在函数定义允许的情况下不传入有默认值的参数，此时未传入实参的形参的值就是默认值。

定义默认值很简单，直接在参数列表通过 `=` 对参数赋值即可。

```python
def foo2(a,b=2):
    print(a,b)
    return

foo(114)
foo(114,514)
"""
114 2
114 514
"""
```

> `Python` 函数有默认值的参数必须放在参数列表的最后。

```python
def foo(a,b,c=1,d=2)#正确
def foo(a,b=1,c=2,d)#错误
```

### 参数列表

有时候函数可能会面临传入未知数目参数的情况，可能是几个也可能是几十个。为了应对这种情况，`Python` 提供了两种特殊的参数标识符： 参数列表 `*` 和关键字参数列表 `**` ，用于标记参数，并将传入的参数打包存入有这些标记的参数中。

1. `*args`

```python
def foo(a,*args):#args即为参数列表，名称可以自定义，会将收到的参数打包为元组
    print(a,args)
    return

foo(1,2,3,4,5,6,7,8)
foo(1)
"""
1 (2, 3, 4, 5, 6, 7, 8)
1 ()
"""
```

> 在 `*args` 参数列表后出现的普通参数必须通过指定形参来传参。

```python
def foo(a,b,c=1,*args)#正确
def foo(a,*args,c)
foo(1,2,3,4,5,c=6)#正确
foo(1,2,3,4,5,6)#错误
```

2. `**kwargs`

在指定要传入的形参时，除了可以指派给位于参数列表的形参，也可以指派给不存在的形参。

```python
def foo(a,*args,**kwargs):#kwargs即为关键字参数列表，名称可以自定义，会将收到的参数打包为字典
    print(a,args,kwargs)
    return

foo(1,2,3,4,5,6,7,8,b=15,c=16,d=17,e=18,f=19,g=20,h=21,i=22,j=23)
foo(1,2)
"""
1 (2, 3, 4, 5, 6, 7, 8) {'b': 15, 'c': 16, 'd': 17, 'e': 18, 'f': 19, 'g': 20, 'h': 21, 'i': 22, 'j': 23}
1 (2,) {}
"""
```

> `**kwargs` 只允许出现在参数列表的最后，与 `*args` 同时使用时其之间的参数只能通过指定的方式传参。

```python
def foo(a,*args,b=1,c,**kwargs):
    print(a,args,kwargs)
    return

foo(1,2,3,4,5,6,7,8,b=15,c=16,d=17,e=18,f=19,g=20,h=21,i=22,j=23)
```

### 参数拆包

`*` 和 `**` 标记除了可以在参数列表标记特殊参数，也可以用于在传参时拆包集合类型。`*` 会展开集合类型；`**` 会将字典中键值对的 `值` 传入 `键` 对应的形参，即 `key = value`。

```python
args = [1,2,3,4]
kwargs = {'e':5,'f':6}

foo(*args,114,homo=514,**kwargs)
#等价于：
foo(1,2,3,4,114,homo=514,e=5,f=6)
```

## 函数的返回值

函数可以通过 `return` 返回一个返回值。

```python
def add(a,b):
    return a+b

print(add(114,514))
"""
628
"""
```

使用 `return` 返回值后，函数即结束执行，后续所有语句都会被忽略。如果只想结束函数的执行，可以直接 `return` 而不返回值，此时返回值为 `None`。

> 函数存在返回值时，整个被调用函数为右值表达式。

## 匿名函数

除了使用 `def` 关键字定义的有自己名称的函数，还有另一种没有名称的函数，只有参数列表、函数体和返回值，称为匿名函数，又称Lambda表达式。

### 匿名函数的定义

定义匿名函数使用 `lambda` 关键字。

```python
lambda paramters:expr
#expr为单句表达式，同时作为匿名函数的返回值。
lambda x,y,z:x + y + z
```

### 使用匿名函数

匿名函数可以像普通函数一样使用，也可以赋值给其他变量然后使用。

```python
foo = lambda x,y,z: x + y + x
print(foo(1,2,3))
print((lambda x,y,z: x + y + x)(1,2,3))
"""
6
6
"""
```

通常匿名函数不是直接拿来使用的，而是其他函数需要传入一个函数作为参数时，传入匿名函数而不用大费周章的使用 `def` 定义函数再传入。

## 装饰器

装饰器如其名，用于给函数外挂一些东西，从而修改函数的行为。

### 装饰器的定义

装饰器是一类特殊的函数，接受一个函数作为参数并返回一个函数，传入的函数即为被包装的函数。

```python
def decorator_func(target_func):#装饰器函数
    def wrapper(*args, **kwargs):#返回的函数，args和kwargs即为原函数接收的参数
        #代码
        result = target_func(*args, **kwargs)
        #代码
        return result
    return wrapper

def my_decorator(origin_func):
    def wrapper(*args, **kwargs):#返回的函数，args和kwargs即为原函数接收的参数
        print(args,kwargs)
        args = args + (4,5,6)#篡改参数
        result = origin_func(*args, **kwargs)
        print("Decorated")
        return result
    return wrapper
```

### 使用装饰器

装饰器使用在函数定义的位置，在函数定义的 `def` 语句的上一行，以 `@` 开头。

```python
@decorator
def foo():
    pass

def my_decorator(origin_func):
    def wrapper(*args, **kwargs):#返回的函数，args和kwargs即为原函数接收的参数
        print("Original Arguments")
        print(args,kwargs)
        args = args + (4,5,6)#篡改参数
        result = origin_func(*args, **kwargs)
        print("Decorated")
        return result
    return wrapper

@my_decorator
def func(a,b,*args,**kwargs):
    print(a,b,args,kwargs)

func(1,2,3,4,e=7,f=8,g=9)
#等价于my_decorator(func)(*args,**kwargs)
"""
Original Arguments
(1, 2, 3, 4) {'e': 7, 'f': 8, 'g': 9}
1 2 (3, 4, 4, 5, 6) {'e': 7, 'f': 8, 'g': 9}
Decorated
"""
```

### 带参数的装饰器

装饰器可以带参数，对应的函数需要多一层。

```python
def my_decorator(x,y,z):#xyz即装饰器的参数
    def decorate(origin_func):
        def wrapper(*args, **kwargs):#返回的函数，args和kwargs即为原函数接收的参数
            print("Original Arguments")
            print(args,kwargs)
            args = args + (x,y,z)#篡改参数
            result = origin_func(*args, **kwargs)
            print("Decorated")
            return result
        return wrapper
    return decorate

@my_decorator(11,22,33)
def func(a,b,*args,**kwargs):
    print(a,b,args,kwargs)

func(1,2,3,4,e=7,f=8,g=9)
#等价于my_decorator(11,22,33)(func)(*args,**kwargs)
#      my_decorator^ decorate^    wrapper^
"""
Original Arguments
(1, 2, 3, 4) {'e': 7, 'f': 8, 'g': 9}
1 2 (3, 4, 11, 22, 33) {'e': 7, 'f': 8, 'g': 9}
Decorated
"""
```

### 类装饰器

装饰器不仅可以是一个函数，也可以是一个类，此处将会在 [类与对象](../python-class-and-object) 章节介绍。

## 写在最后

装饰器可以动态修改函数的行为，记录函数的调用信息、拦截特定的调用、限制函数访问权限等功能而不用修改函数本身，是 `Python` 中十分强大的一个特性。
