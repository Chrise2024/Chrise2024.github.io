---
title: Python-字符串补充
description: String Addon
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-03 16:42 +0800
nav:
  prev:
    path: posts/python/python-tuple-and-string
    title: Python-运算符与数字
  next:
    path: posts/python/python-collection
    title: Python-集合类型
--- 

# 字符串方法

字符串提供了许多内建的方法，可以为使用提供许多便利。熟悉这些方法，避免重复造轮子。

1. `capitalize() -> str`

返回一个将原字符串的第一个字符转换为大写的新字符串，原字符串不改变。

```python
#Python
str1 = "hello,world"
print(str1.capitalize())
"""
Hello,world
"""
```

2. `center(width:int [, fillchar:str = ' ']) -> str`

返回一个指定宽度的字符串并将原字符串置于新字符串中间。缺位用`fillchar`补齐，默认为空格。若宽度小于原字符串宽度则不会做任何处理（会返回原字符串）。

```python
#Python

str1 = "ABCDEFG"
print(str1.center(2))
print(str1.center(20))
print(str1.center(20,'='))
"""
ABCDEFGH
      ABCDEFGH      
======ABCDEFG=======
"""
```

3.	`count(str:str [, beg:int = 0][, end:int = len(string)]) -> int`

返回`str`在字符串里面出现的次数。

> `beg`：寻找的起始位置<br>`end`：寻找的终止位置



```python
#Python

str1 = "Hello,World"
print(str1.count('o'))
print(str1.count('o',7))
print(str1.count('o',0,6))
"""
2
1
1
"""
```

4. `encode([encoding:str = 'UTF-8'][,errors:str = 'strict']) -> bytes`

指定的编码格式编码字符串。

```python
#Python

str1 = "��������"
print(str1.encode('utf-8'))

print(str1.encode('utf-8').decode('gbk'))
"""
b'\xef\xbf\xbd\xef\xbf\xbd\xef\xbf\xbd\xef\xbf\xbd\xef\xbf\xbd\xef\xbf\xbd\xef\xbf\xbd\xef\xbf\xbd'
锟斤拷锟斤拷锟斤拷锟斤拷
"""
```

支持的编码（均忽略大小写）：

- `UTF-8`，可变长度的编码，可以表示`Unicode`字符集中的所有字符，兼容`ASCII`

- `ASCII`，7二进制位表示128个字符

- `GBK`，汉字编码标准，用于表示简体中文字符。

- `Big5`，繁体中文编码标准，用于表示繁体中文字符。

- `Latin-1 (ISO-8859-1)`，用于西欧语言的编码标准，包含一些特殊字符，如 á、é、ç 等。

</Details>
<br>

5. `endswith(suffix:str [, start:int][, end:int]) -> bool`

检查字符串是否以`suffix`结尾。

> `start`：结尾判定的起始位置<br>`end`：结尾判定的终止位置

```python
#Python

str1 = "OHHHHHHHHHH!"
print(str1.endswith('HH!'))
"""
True
"""
```

6. `expandtabs([tabsize:int = 8]) -> str`

把字符串中的水平制表符转为空格。默认水平制表符长度为8空格，当前位置到开始位置或上一个制表符位置的字符数不足 8 的倍数则以空格代替。返回新字符串，原字符串不受影响。

> `tabsize`：制表符长度

```python
#Python

str1 = "Hello\tWorld"
print('Default:', str1.expandtabs())
print('tabsize = 2:', str1.expandtabs(2))
print('tabsize = 3:', str1.expandtabs(3))
print('tabsize = 4:', str1.expandtabs(4))
print('tabsize = 5:', str1.expandtabs(5))
print('tabsize = 6:', str1.expandtabs(6))
"""
Default: Hello   World
tabsize = 2: Hello World
tabsize = 3: Hello World
tabsize = 4: Hello   World
tabsize = 5: Hello     World
tabsize = 6: Hello World
"""
```

7. `find(str:str [, beg:int = 0][, end:int = len(string)]) -> int`

查找`str`在字符串内第一次出现位置的索引，未找到则返回-1。

> `beg`：寻找的起始位置<br>`end`：寻找的终止位置

```python
#Python

str1 = "Hello,World"
str2 = "or"
 
print (str1.find(str2))
print (str1.find(str2, 5))
print (str1.find(str2, 1, 6))
"""
7
7
-1
"""
```

8. `index(str:str [, beg:int = 0][, end:int = len(string)]) -> int`

同`find`，只不过在未找到时会抛异常。

9. `isalnum() -> bool`

检查字符串是否由字母和数字组成，即字符串中的所有字符都是字母或数字。

```python
#Python

str1 = "Hello,World"
str2 = "Hello World"
str3 = "HelloWorld"
print (str1.isalnum())
print (str2.isalnum())
print (str3.isalnum())
"""
False
False
True
"""
```

10. `isalpha() -> bool`

检测字符串是否只由字母或文字组成。

```python
#Python

str1 = "Hello,World"
str2 = "Hello你好"
print (str1.isalpha())
print (str2.isalpha())
"""
False
True
"""
```

11. `isdecimal() -> bool`

检测字符串是否只由十进制数字组成。

```python
#Python

str1 = "114514"
str2 = "1E2F"
print (str1.isdecimal())
print (str2.isdecimal())
"""
True
False
"""
```

12. `isdigit() -> bool`

检测字符串是否只由数字组成。

```python
#Python

str1 = "114514"
str2 = "homo114514"
print (str1.isdigit())
print (str2.isdigit())
"""
True
False
"""
```

13. `islower() -> bool`

检测字符串是否只由小写字符组成（忽略不存在大小写的字符）。

```python
#Python

str1 = "Homo114514"
str2 = "homo114514"
print (str1.islower())
print (str2.islower())
"""
True
False
"""
```

14. `isnumeric() -> bool`

 方法检测字符串是否只由数字组成，数字可以是：`Unicode`数字，全角数字（双字节），罗马数字，汉字数字。

```python
#Python

#str1 = '²1234'
str1 = '\u00B21234'
print(str1.isnumeric())
# str1 = '½'
str1 = '\u00BD'
print(str1.isnumeric())

str2 = "十二"#汉字数字
print(str2.isnumeric())

str2 = "陆"#大写汉字数字
print(str2.isnumeric())

str3 = "Ⅳ"#罗马数字
print(str3.isnumeric())
"""
True
True
True
True
True
"""

```

15. `isspace() -> bool`

检测字符串是否只由空白字符组成。

> 空格、换行符`\n`、回车符`\r`、水平制表符`\t`均被视为空白字符

```python
#Python

str1 = "     \n\t\r"
str2 = "sbksbk   bsvbs"
str3 = ""
print(str1.isspace())
print(str2.isspace())
print(str3.isspace())
"""
True
False
False
"""
```

16. `istitle() -> bool`

检测字符串中所有的单词拼写首字母是否为大写，且其他字母为小写（标题格式）。

```python
#Python

str1 = "This Is An Example"
str2 = "This is an example"
print (str1.istitle())
print (str2.istitle())
"""
True
False
"""
```

17. `isupper() -> bool`

方法检测字符串中所有的字母是否都为大写（忽略不存在大小写的字符）。

```python
#Python

str1 = "UPPER666"
str2 = "Upper123"
str3 = "114514"
print(str1.isupper())
print(str2.isupper())
print(str3.isupper())
"""
True
False
False
"""
```

18. `join(iterable:iterable) -> str`

将序列中的元素以原字符串连接生成一个新的字符串。

```python
#Python

seq = ("1","1","4","5","1","4")
str1 = "="
print(str1.join(seq))
"""
1=1=4=5=1=4
"""
```

19. `ljust(width:int [, fillchar:str = ' '])`

返回一个左对齐的原字符串，左侧宽度不足则用`fillchar`补齐。如果指定的长度小于原字符串的长度则返回原字符串。

```python
#Python

str1 = "Hello"
print (str1.ljust(10, '+'))
"""
Hello+++++
"""
```

20. `lower() -> str`

将原字符串的大写字符转为小写。返回新字符串，原字符串不变。

```python
#Python

str1 = "HELLO"
print(str1.lower())
"""
hello
"""
```

21. `lstrip([char:str = ' ']) -> str`

用于截掉字符串左边的空格或指定字符。

```python
#Python

str1 = "     114514"
str2 = "66666666这个入是桂"
print(str1.lstrip())
print(str2.lstrip('6'))
"""
114514
这个入是桂
"""
```

22. `maketrans(x:str, y:str [, z:str]) -> str` or `maketrans(x:dict) -> dict`

根据特定规则映射字符串中的字符，返回一个字典，包含对应的映射关系。

> `z`：要删除的字符，字符串会被当做字符的集合

```python
#Python

str1 = "Hello,World"
print(str1.maketrans("H","N"))
print(str1.maketrans("aeiou","12345"))#使用字符串映射必须两个字符串等长，a会被映射成1，以此类推
print(str1.maketrans({"l":6},"d"))
```

23. `translate(table:dict) -> str`

根据`maketrans`得到的映射表或手动给出的映射表，转换字符串。

```python
#Python

str1 = "Hello,World"
trans = str1.maketrans("H","N")
print(str1.translate(trans))
"""
Nello,World
"""
```

24. `replace(old:str, new:str[, max:int]) -> str`

返回一个把原字符串中的`old`（旧字符串）替换成`new`（新字符串）的新字符串，原字符串不变。

> `max`指定替换次数的上限，未指定则无上限

```python
#Python

str1 = "Hello,World"
print(str1.replace("World","Python"))
print(str1.replace("o","O",1))
"""
Hello,Python
HellO,World
"""
```

25. `rfind(str:str[, beg:int = 0][, end:int = len(string)])`

查找`str`在字符串内第一次出现位置的索引，未找到则返回-1。

整体类似`find`，只不过是从右开始查找。

```python
#Python

str1 = "Hello,World"
str2 = "or"
 
print (str1.rfind(str2))
print (str1.rfind(str2, 5))
print (str1.rfind(str2, 1, 6))
"""
7
7
-1
"""
```

26. `rindex(str:str [, beg:int = 0][, end:int = len(string)]) -> int`

类似`index`，从右侧开始查找。


27. `rjust(width:int [, fillchar:str = ' '])`

类似`ljust`，只不过是右对齐，填充字符在左侧。

28. `rstrip([char:str = ' ']) -> str`

去除原字符串右侧的空格或指定字符，使用方法同`lstrip`。

29. `split([str:str = ""][, num:int = string.count(str)])`

在原字符串的`str`位置切割，会移除`str`。

> `num`指定最大切割次数，未指定则不限制。

```python
#Python

str1 = "I will never gonna give you up"
print(str1.split(' '))
print(str1.split('e',2))
"""
['I', 'will', 'never', 'gonna', 'give', 'you', 'up']
['I will n', 'v', 'r gonna give you up']
"""
```

30. `splitlines([keepends:bool = False])`

按照行切割字符串（以换行符`\n`、回车符`\r`作为行分隔符）。

> `keepends`控制是否保留换行符

```python
#Python

str1 = "ab c\n\nde fg\rkl\r\n"
print(str1.splitlines())
print(str1.splitlines(True))
"""
['ab c', '', 'de fg', 'kl']
['ab c\n', '\n', 'de fg\r', 'kl\r\n']
"""
```

31. `startswith(prefix:str [, start:int][, end:int]) -> bool`

检查字符串是否以`prefix`开头。

> `start`：开头判定的起始位置<br>`end`：开头判定的终止位置

```python
#Python

str1 = "OHHHHHHHHHH!"
print(str1.endswith('OH'))
"""
True
"""
```

32. `strip([char:str]) -> str`

在字符串上同时执行`lstrip`和`rstrip`

33. `swapcase() -> str`

将字符串中大写转换为小写，小写转换为大写.

```python
#Python

str1 = "AbCdeFG"
print(str1.swapcase())
"""
aBcDEfg
"""
```

34. `title() -> str`

将字符串转为标题格式。

```python
#Python

str1 = "hello world"
print(str1.title())
"""
Hello World
"""
```

35. `upper() -> str`

将原字符串的小写字符转为大写。返回新字符串，原字符串不变。

```python
#Python

str1 = "hello"
print(str1.upper())
"""
HELLO
"""
```

36. `zfill(width:int) -> str`

返回指定宽度的右对齐字符串，左侧缺位用0补齐，指定宽度小于原字符串长则返回原字符串。

等效于`rjust(width,'0'])`

```python
#Python

str1 = "Hello"
print(str1.zfill(10))
"""
00000Hello
"""
```
