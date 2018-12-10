---
title: Python安装及基础知识
date: 2018-06-12 09:28:26
tags: python
categories: 技术
---

## 一、[版本切换]()
#### `mac os` 自带 `Pyhton 2.7` ，如想使用 `Python 3` ，请前往官网自行下载安装
#### [官网地址](https://www.python.org/)
#### [python3 英文文档](https://docs.python.org/3/)
#### [python3 中文文档](http://www.pythondoc.com/pythontutorial3/stdlib.html#tut-os-interface)
#### 终端输入 `python` 可以看到版本号等信息
#### 终端输入 `python3`
<!-- more -->
#### 设置别名
```
$ vi ~/.bash_profile 
$ alias py2='python'
$ alias py3='python3'
```
---
## 二、[输入和输出]()
### `Python` 的交互式命令行
- 在终端键入 `python` 进入交互模式
- `exit()` 退出交互模式

### `print()` 输出
##### 打印字符串
```
# 多个字符串，用逗号“,”隔开，就可以连成一串输出。遇到逗号“,”会输出一个空格。
$ print('The quick brown fox', 'jumps over', 'the lazy dog')
# The quick brown fox jumps over the lazy dog

>>> print('I\'m learning\nPython.')
# I'm learning
Python.
```
##### 打印整数
```
>>> print('100 + 200 =', 100 + 200)
# 100 + 200 = 300
```

##### 转义符
```
>>> print('\\\t\\')
# \       \

# r''表示''内部的字符串默认不转义
>>> print(r'\\\t\\')
# \\\t\\
```

##### 多行
```
#'''...'''的格式表示多行内容
>>> print('''line1
... line2
... line3''')
# line1
line2
line3
```

##### 混合
```
>>> print(r'''hello,\n
world''')
# hello,\n
world
```

### `input()` 输入
```
>>> name = input()
> Jesse
>>> name
# Jesse
```
---
## 三、[数据类型]()
### 整数
- `1`，`100`，`-8080`，`0`
- 整数的除法
```
>>> 10 / 3
# 3.3333333333333335

>>> 9 / 3
# 3.0

# 地板除
>>> 10 // 3
# 3

# 余数运算
>>> 10 % 3
# 1
```

### 浮点数
- `1.23`乘以 `10` 的 `9` 次方 就是 `1.23e9`
- `0.000012`可以写成`1.2e-5`

### 字符串
```
>>> print('I\'m \"OK\"!')
# I'm "OK"!
```

### 布尔值
##### `True`

##### `False`

##### 布尔值可以用 `and` 、`or` 和 `not` 运算
```
>>> True and False
False
>>> not 1 > 2
True
```

##### 条件判断
```
if age >= 18:
    print('adult')
else:
    print('teenager')
```

### 空值 `None`

### 常量
```
// 常量就是不能变的变量
PI = 3.14159265359
```

### 数据类型转换
```
>>> int('123')
# 123

>>> int(12.34)
# 12

>>> float('12.34')
# 12.34

>>> str(1.23)
# '1.23'

>>> str(100)
# '100'

>>> bool(1)
# True

>>> bool('')
# False
```

### `isinstance()` 数据类型检查
```
>>> isinstance('abc', str)
True

>>> isinstance(1.23, (int, float))
True

>>> isinstance([1,2], list)
True

>>> isinstance((1,2), tuple)
True
```
---
## 四、[变量]()
### 动态语言
```
a = 1
a = 'ABC'
```
##### 字符串 `replace()` 方法
```
>>> a = 'abc'
>>> b = a.replace('a', 'A')
>>> b
# 'Abc'
>>> a
# 'abc'
```

### 静态语言
```
int a = 123; // a是整数类型变量
a = "ABC"; // 错误：不能把字符串赋给整型变量
```

### 理解变量在计算机内存中的表示
```
a = 'ABC'
```
  - 在内存中创建了一个'ABC'的字符串
  - 在内存中创建了一个名为 `a` 的变量，并把它指向 `'ABC'`

### 重点
```
a = 'ABC'
b = a
a = 'XYZ'
print(b)
# ABC
```
---
## 五、[`list` 和 `tuple`]()
### `list`
##### Python内置的一种数据类型是列表, list是一种有序的集合
```
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
# ['Michael', 'Bob', 'Tracy']
```

##### 索引
```
>>> classmates[0]
# 'Michael'

>>> classmates[3]
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# IndexError: list index out of range

>>> classmates[-1]
# 'Tracy'

>>> classmates[-2]
'Bob'
```

##### `len`
```
>>> len(classmates)
# 3
```

##### `append` 末尾追加元素
```
>>> classmates.append('Adam')
>>> classmates
# ['Michael', 'Bob', 'Tracy', 'Adam']
```

##### `insert` 在指定位置插入
```
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

##### `pop` 删除
```
# 末尾删除
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']

# 指定位置删除
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

##### 替换元素
```
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

##### `sort()`排序
```
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a
['a', 'b', 'c']
```

### `tuple`
##### `tuple` 和 `list` 非常类似，但是 `tuple` 一旦初始化就不能修改
```
>>> classmates = ('Michael', 'Bob', 'Tracy')
```
##### 定义一个只有 `1` 个元素的 `tuple`
```
>>> t = (1)
>>> t
# 1 整数

>>> t = ('a',)
>>> t
# ('a',) tuple
```

##### 可变的 `tuple`
```
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
# ('a', 'b', ['X', 'Y'])
```
---
## 六、[循环]()
### `for x in ...`
```
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```
### `range()`
```
# 生成一个整数序列
>>> range(5)
# [0, 1, 2, 3, 4]

# `list()` 将 `range()` 转换为 `list`
>>> list(range(5))
# [0, 1, 2, 3, 4]
```
### `while` 循环
```
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```
### `break`
##### break语句可以提前退出循环
```
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')
```
### `continue`
##### 跳过当前的这次循环，直接开始下一次循环
```
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```
### 要特别注意
- 不要滥用break和continue语句。break和continue会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到break和continue语句，上面的两个例子，都可以通过改写循环条件或者修改循环逻辑，去掉break和continue语句。
---
## 七、[`dict` 和 `set`]()
```
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
# 95

>>> d['Adam'] = 67
>>> d['Adam']
# 67

# 访问不存在的 key 会报错
>>> d['Thomas']
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# KeyError: 'Thomas'
```
### `dist`
##### `in` 判断 `key` 是否存在
```
>>> 'Thomas' in d
# False
```

##### `get()`
```
# get() 函数返回指定键的值，如果值不在字典中返回默认值
$ dict.get(key, default=None)

>>> d.get('Thomas')
# None, 默认值

>>> d.get('Thomas', -1)
# -1，指定值
```

##### `pop()` 删除 `key`
```
>>> d.pop('Bob')
# 75
>>> d
# {'Michael': 95, 'Tracy': 85}
```
dict内部存放的顺序和key放入的顺序是没有关系的

##### 和 `list` 比较，`dict` 有以下几个特点
- 查找和插入的速度极快，不会随着key的增加而变慢
- 需要占用大量的内存，内存浪费多
- `list` 相反
- `dict` 是用空间来换取时间的一种方法

### `set`
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
##### 创建
```
>>> s = set([1, 2, 3])
>>> s
# {1, 2, 3}
```
##### 重复元素在 `set` 中自动被过滤
```
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
# {1, 2, 3}
```
##### `add(key)` 添加元素
```
>>> s.add(4)
>>> s
# {1, 2, 3, 4}

>>> s.add(4)
>>> s
# {1, 2, 3, 4}
```
##### `remove(key)` 删除元素
```
>>> s.remove(4)
>>> s
# {1, 2, 3}
```
##### 交集、并集
```
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])

>>> s1 & s2
# {2, 3}

>>> s1 | s2
# {1, 2, 3, 4}
```
---
## 八、[函数]()
### 调用函数
##### `abs()` 
```
>>> abs(100)
# 100
>>> abs(-20)
# 20
>>> abs(12.34)
# 12.34
```
##### `max()`
```
>>> max(1, 2)
# 2
>>> max(2, 3, 1, -5)
# 3
```
##### `min()`
```
>>> max(1, 2)
# 1
>>> max(2, 3, 1, -5)
# -5
##### `filter()` 过滤序列
```
$ filter(function, iterable)

def is_odd(n):
    return n % 2 == 1
 
newlist = filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
print(newlist)
# [1, 3, 5, 7, 9]
# python2中返回的是过滤后的列表list, 而python3中返回到是一个filter类
```
```
##### `lambda` 匿名函数
```
func=lambda x:x+1
print(func(1))
# 2

foo = [2, 18, 9, 22, 17, 24, 8, 12, 27]
print (list(filter(lambda x: x % 3 == 0, foo)))
# [18, 9, 24, 12, 27]
```
### 定义函数
```
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```
##### 引入函数
你已经把my_abs()的函数定义保存为abstest.py文件了
```
from abstest import my_abs
```
##### 空函数
如果想定义一个什么事也不做的空函数，可以用pass语句：
```
def nop():
    pass
```
pass语句什么都不做，那有什么用？实际上pass可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。
##### 参数检查
```
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```
##### 返回多个值
```
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
# 151.96152422706632 70.0
# 但其实这只是一种假象，Python函数返回的仍然是单一值：
>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```
### 函数的参数
##### 位置参数
```
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

>>> power(5, 2)
# 25
```
##### 默认参数
```
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

>>> power(5)
# 25
```
- 注意
```
def add_end(L=[]):
    L.append('END')
    return L

>>> add_end()
# ['END']
>>> add_end()
# ['END', 'END']
```
- Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。
- 定义默认参数要牢记一点：默认参数必须指向不变对象！

```
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L

>>> add_end()
# ['END']
>>> add_end()
# ['END']
```
##### 可变参数
- `list` 或 `tuple`
```
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

>>> calc([1, 2, 3])
# 14
>>> calc((1, 3, 5, 7))
# 84
```
- 可变参数
```
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

>>> calc(1, 2)
# 5

>>> calc()
# 0

>>> nums = [1, 2, 3]
>>> calc(*nums)
# 14
```
##### 关键字参数
```
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

>>> person('Michael', 30)
# name: Michael age: 30 other: {}

>>> person('Bob', 35, city='Beijing')
# name: Bob age: 35 other: {'city': 'Beijing'}

>>> person('Adam', 45, gender='M', job='Engineer')
# name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
# name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```
##### 命名关键字参数
必须要传的参数，`*` 后面的参数被视为命名关键字参数
```
def person(name, age, *, city, job):
    print(name, age, city, job)

>>> person('Jack', 24, city='Beijing', job='Engineer')
# Jack 24 Beijing Engineer
```
##### 参数组合
```
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```
```
>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
```
```
>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}
>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}
>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```
### 递归函数
```
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)

>>> fact(5)
# 120  => 5 * 4 * 3 * 2 * 1
```
##### 栈
- 递归函数的优点是定义简单，逻辑清晰。理论上，所有的递归函数都可以写成循环的方式，但循环的逻辑不如递归清晰。
- 使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。可以试试fact(1000)：

```
>>> fact(1000)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 4, in fact
  ...
  File "<stdin>", line 4, in fact
RuntimeError: maximum recursion depth exceeded in comparison
```
- 解决递归调用栈溢出的方法是通过尾递归优化，事实上尾递归和循环的效果是一样的，所以，把循环看成是一种特殊的尾递归函数也是可以的。

##### 尾递归
```
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```
```
>>> fact(5)

===> fact_iter(5, 1)
===> fact_iter(4, 5)
===> fact_iter(3, 20)
===> fact_iter(2, 60)
===> fact_iter(1, 120)
===> 120
```