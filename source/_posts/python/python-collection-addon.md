---
title: Python-集合类型补充
description: Collection Addon
tags: [Programing,Python,Python-Tutorial]
categories: Programing
math: true
lang: zh-CN
date: 2025-02-06 16:42 +0800
nav:
  prev:
    path: posts/python/python-collection
    title: Python-集合类型
  next:
    path: posts/python/python-decision-statement
    title: Python-判断语句
--- 

# 列表方法

列表提供了许多内建的方法，用来快速操作列表。

1. `append(obj:any) -> None`

用于在列表末尾添加新的对象。

```python
#Python

list1 = [1,2,3]
list1.append(4)
print(list1)
"""
[1, 2, 3, 4]
"""
```

2. `clear() -> None`

清空列表。

```python
#Python

list1 = [1,2,3]
list1.clear()
print(list1)
"""
[]
"""
```

3. `copy() -> list`

创建一个当前列表的浅拷贝。

```python
#Python

list1 = [1,2,3]
list2 = list1.copy()
print(id(list1) == id(list2))
"""
False
"""
```

4. `count(obj:any) -> int`

用于统计某个元素在列表中出现的次数。

```python
#Python

list1 = [1,2,3]
print(list1.count(4),list1.count(3))
"""
0 1
"""
```

5. `extend(seq:iterable) -> None`

将新序列追加到列表末尾。

> 如果序列为字典类型则只追加字典的键

```python
#Python

list1 = [1,2,3]
list1.extend([4,5,6])
print(list1)
"""
[1, 2, 3, 4, 5, 6]
"""
```

6. `index(obj:any [, start:int = 0][, stop:int = 9223372036854775807]) -> int`

返回`obj`在列表内第一次出现位置的索引，不存在则引发`ValueError`异常。

```python
#Python

list1 = [1,1,4,5,1,4]
 
print(list1.index(5))
print(list1.index(5, 3))
print(list1.index(5, 1, 3))
"""
3
3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: 5 is not in list
"""
```

7. `insert(index:int, obj:any) -> None`

用于将指定对象插入列表的指定位置。

```python
#Python

list1 = [1,1,4,5,1,4]

list1.insert(2,10086)
print(list1)
"""
[1, 1, 10086, 4, 5, 1, 4]
"""
```

8. `pop([index:int = -1]) -> any`

移除列表指定位置的元素并返回这个元素。

```python
#Python

list1 = [1,1,4,5,1,4]

print(list1.pop(2))
print(list1)
"""
4
[1, 1, 5, 1, 4]
"""
```

9. `remove(obj:any) -> None`

用于移除列表中第一个出现的`obj`，若`obj`不存在于列表中则引发`ValueError`异常。

```python
#Python

list1 = [1,1,4,5,1,4]

list1.remove(5)
print(list1)
list1.remove(9)
"""
[1, 1, 4, 1, 4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
"""
```

10. `reverse() -> None`

反向列表中的元素。

```python
#Python

list1 = [1,1,4,5,1,4]

list1.reverse()
print(list1)
"""
[4, 1, 5, 4, 1, 1]
"""
```

11. `sort([key:function = None][, reverse:bool = False]) -> None`

用于排序列表中的元素。

> `key`是排序使依据，默认为`None`，即使用比较运算符的结果作为依据。<br>`reverse`为排序的规则，`True`为降序排列，`False`为升序排列。

```python
#Python

def sort_key(x):
    return (x-4)**2

list1 = [1,1,4,5,1,4]
list1.sort()
print(list1)
list1.sort(key=sort_key)
print(list1)
list1.sort(reverse=True)
print(list1)
"""
[1, 1, 1, 4, 4, 5]
[4, 4, 5, 1, 1, 1]
[5, 4, 4, 1, 1, 1]
"""
```

# 字典方法

1. `clear() -> None`

清空字典。

```python
#Python

dict1 = {1:2,3:4}
dict1.clear()
print(dict1)
"""
{}
"""
```

2. `copy() -> list`

创建一个当前字典的浅拷贝。

```python
#Python

dict1 = {1:2,3:4}
dict2 = dict1.copy()
print(id(dict1) == id(dict2))
"""
False
"""
```

3. `get(key:any[, default = None])`

获取`key`对应的值，键不存在则返回`default`。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.get(1145,"666"))
print(dict1)
"""
666
{1: 2, 3: 4}
"""
```

4. `items() -> dict_items`

返回一个可迭代的包含字典所有键值对的集合，可转为`list`。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.items())
print(list(dict1.items()))
"""
dict_items([(1, 2), (3, 4)])
[(1, 2), (3, 4)]
"""
```

5. `keys() -> dict_keys`

返回一个可迭代的包含字典所有键的集合，可转为`list`。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.keys())
print(list(dict1.keys()))
"""
dict_keys([1, 3])
[1, 3]
"""
```

6. `pop(key:any[, default:any])`

用于移除字典内`key`键及其对应的值并返回这个值。

> `default`指定键不存在时的返回值。若未指定`default`且键不存在则引发`KeyError`异常。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.pop(3))
print(dict1)
"""
4
{1: 2}
"""
```

7. `popitem() -> tuple`

删除最后一对键值对并返回这对键值对。若字典为空则引发`KeyError`异常。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.popitem())
print(dict1)
"""
(3, 4)
{1: 2}
"""
```

8. `setdefault(key:any[, default:any = None]) -> any`

如果`key`存在于字典中则返回对应的值，不存在则创建键并赋值为`default`，返回`default`。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.setdefault(1145,"666"))
print(dict1)
"""
666
{1: 2, 3: 4, 1145: '666'}
"""
```

9. `update(dict2:dict) -> None`

将字典`dict2`合并入原字典并覆盖原有的值。

```python
#Python

dict1 = {1:2,3:4}
dict1.update({1:5,114:514})
print(dict1)
"""
{1: 5, 3: 4, 114: 514}
"""
```
10. `values() -> dict_values`

返回一个可迭代的包含字典所有值的集合，可转为`list`。

```python
#Python

dict1 = {1:2,3:4}
print(dict1.values())
print(list(dict1.values()))
"""
dict_values([2, 4])
[2, 4]
"""
```

# 集合方法

集合提供了许多内建的方法，用来快速操作集合。

1. `add(elem:any) -> None`

用于向集合中添加元素。若元素已经存在则不会有任何动作。

```python
#Python

set1 = {1,2,3,4}
set1.add(5)
set1.add(4)
print(set1)
"""
{1, 2, 3, 4, 5}
"""
```

2. `clear() -> None`

清空集合。

```python
#Python

set1 = {1,2,3,4}
set1.clear()
print(set1)
"""
{}
"""
```

3. `copy() -> list`

创建一个当前集合的浅拷贝。

```python
#Python

set1 = {1,2,3,4}
set2 = set1.copy()
print(id(set1) == id(set2))
"""
False
"""
```

4. `difference(set2:set) -> set`

返回集合与集合`set2`的差集（A-B）。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}
print(set1.difference(set2))
"""
{1}
"""
```

5. `difference_update(set2:set) -> None`

用于移除两个集合中都存在的元素（$\complement_{A \cup B}B$）。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}
set1.difference(set2)
print(set1)
"""
{1}
"""
```

6. `discard(value:any) ->None`

用于移除集合中指定元素。元素不存在不会引发异常。

```python
#Python

set1 = {1,2,3,4}
set1.discard(1)
set1.discard(5)
print(set1)
"""
{2, 3, 4}
"""
```

7. `intersection(set2:set,set3:set....setn:set) -> set`

返回集合与集合`set2`或多个集合之间的交集（$A \cap B$）。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}

print(set1.intersection(set2))
"""
{2, 3, 4}
"""
```

8. `intersection_update(set2:set,set3:set....setn:set) -> None`

用于移除不都存在与所有集合中的元素，即保留交集元素。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}

set1.intersection_update(set2)
print(set1)
"""
{2, 3, 4}
"""
```

9. `isdisjoint(set2:set) -> bool`

返回两个集合是否有公共元素（交集）。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}
set3 = {8,9,10}

print(set1.isdisjoint(set2))
print(set1.isdisjoint(set3))
"""
False
True
"""
```

10. `issubset(set2:set) -> bool`

返回集合是否为`set2`的子集。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}
set3 = {1,2,3,4,5}

print(set1.issubset(set2))
print(set1.issubset(set3))
"""
False
True
"""
```

11. `issuperset(set2:set) -> bool`

返回`set2`是否为当前集合的子集。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}
set3 = {1,2,3}

print(set1.issuperset(set2))
print(set1.issuperset(set3))
"""
False
True
"""
```

12. `pop() ->`

随机删除一个元素并返回这个元素。

```python
#Python

set1 = {1,2,3,4}

print(set1.pop())
print(set1)
"""
1
{2, 3, 4}
"""
```

13. `remove(value:any) ->None`

用于移除集合中指定元素。元素不存在会引发`KeyError`异常。

```python
#Python

set1 = {1,2,3,4}
set1.remove(1)
print(set1)
set1.remove(5)
"""
{2, 3, 4}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 5
"""
```

14. `symmetric_difference(set2:set)`

返回两个集合中非公共元素的集合（$\complement_{A \cup B}(A \cap B)$）。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}

print(set1.symmetric_difference(set2))
"""
{1, 5}
"""
```

15. `symmetric_difference_update(set2:set)`

移除两个集合的公共元素并插入另一个集合的非公共元素。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}

set1.symmetric_difference_update(set2)
print(set1)
"""
{1, 5}
"""
```

16. `union(set2:set,set3:set....setn:set) -> set`

返回集合与集合`set2`或多个集合之间的并集（$A \cup B$）。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}

print(set1.union(set2))
"""
{1, 2, 3, 4, 5}
"""
```

17. `update(set2:set)`

合并`set2`入集合。

```python
#Python

set1 = {1,2,3,4}
set2 = {2,3,4,5}

set1.update(set2)
print(set1)
"""
{1, 2, 3, 4, 5}
"""
```