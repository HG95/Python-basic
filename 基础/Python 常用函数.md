# Python 常用函数

## `lambda`函数

Python 中，`lambda` 函数也叫匿名函数，及即没有具体名称的函数，它允许快速定义单行函数，类似于 C 语言的宏，可以用在任何需要函数的地方。
lambda 与 def 的区别：

1. def 创建的方法是有名称的，而 lambda 没有。
2. lambda会返回一个函数对象，但这个对象不会赋给一个标识符，而 def 则会把函数对象赋值给一个变量（函数名）。
3. lambda 只是一个表达式，而def则是一个语句。
   lambda 表达式” : “后面，只能有一个表达式，def 则可以有多个。
4. 像 if 或 for 或 print 等语句不能用于 lambda 中，def 可以。
5. lambda 一般用来定义简单的函数，而def可以定义复杂的函数。
6. lambda 函数不能共享给别的程序调用，def可以。
   

lambda 语法格式：
lambda 变量 : 要执行的语句

```python
lambda [arg1 [, agr2,.....argn]] : expression

```

lambda表达式中，冒号前面是参数，可以有多个，用逗号分隔，冒号右边是返回值。

```python
# 1、单个参数的：
>>> g = lambda x : x ** 2
>>> print g(3)

#2、多个参数的：
>>> g = lambda x, y, z : (x + y) ** z
>>> print g(1,2,2)

```

lambda表达式会返回一个函数对象，如果没有变量接受这个返回值的话，它很快就会被丢弃。也正是由于lambda只是一个表达式，所以它可以直接作为`list`和`dict`的成员。如：

```python
>>> list_a = [lambda a: a**3, lambda b: b**3]
>>> list_a[0]
<function <lambda> at 0x0259B8B0>
>>> g = list_a[0]
>>> g(2)
8

```

## `map函数`

```python
map(func, *iterables) --> map object

参数function传的是一个函数名，可以是python内置的，也可以是自定义的。
参数iterable传的是一个可以迭代的对象，例如列表，元组，字符串这样的。
```

处理序列中的的每个元素，得到的结果是一个‘列表+’，该列表的元素个数及位置与原来的一样；

```python
a=(1,2,3,4,5)
b=[1,2,3,4,5]

la=map(lambda x:x+1,a)
lb=map(lambda x:x**2,b)
print(list(la))
print(list(lb))


输出：
[2, 3, 4, 5, 6]
[1, 4, 9, 16, 25]
```

## `filter函数`

```python
filter(function or None, iterable) --> filter object
```

对 sequence 中的 item 依次执行 function(item)，**将执行结果为True的item组成一个list/String/Tuple（取决于sequence的类型）返回;**

```python
people=[
    {'name':'alex','age':24},
    {'name':'hu','age':23},
    {'name':'gu','age':18}
]

print(list(filter(lambda p:p['age']<=20,people )))
# 输出：[{'name': 'gu', 'age': 18}]

```

## `reduce函数`

```python
reduce(function, sequence[, initial]) -> value
```

- `function` – 函数，要有两个参数

- `iterable` – 可迭代对象

- `initializer` – 可选，初始参数

  reduce() 函数即为化简函数，它的执行过程为：每一次迭代，都将上一次的迭代结果（注：第一次为 init 元素，如果没有指定 init 则为 seq 的第一个元素）与下一个元素一同传入二元 func 函数中去执行。在reduce()函数中，init 是可选的，如果指定，则作为第一次迭代的第一个元素使用，如果没有指定，就取seq中的第一个元素。

  <img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503141147.png"/>

> 需要导入模块 `from functools import reduce`

从 reduce函数的执行过程，让我们很容易联想到求一个数的阶乘，而Python中并没有给出一个求阶乘的内置函数，正好我们就拿这个例子来说明reduce函数吧。

```python
#未指定init的情况
>>> n = 6
>>> print reduce(lambda x, y: x * y, range(1, n))
120

```

上面的例子中range(1,6)函数生成的是一个[1, 2, 3, 4, 5]这样的列表，这里我们给它个名叫seq1吧，reduce()函数执行时，由于没有指定init参数，所以将取seq1中的第一个元素1，作为第一个元素，由于前面的lambda有2个变量，所以需要两个实参，于是就取seq1中的第2个元素2，与第一个元素1一起传入lambda中去执行，并将返回结果2，并同下一个元素3再一起传入lambda中执行，再次返回的结果，作为下一次执行的第一个元素，依次类推，就得出结果5! = 120。
如果我们希望得到阶乘的结果再多增加几倍，可以启用init这个可选项。如：

```python
>>> print reduce(lambda x, y: x * y, range(1, n),2)
240

```

```python

>>>def add(x, y) :            # 两数相加
...     return x + y
... 
>>> reduce(add, [1,2,3,4,5])   # 计算列表和：1+2+3+4+5
15
>>> reduce(lambda x, y: x+y, [1,2,3,4,5])  # 使用 lambda 匿名函数
15

```

## `partial()`偏函数

Python 偏函数是通过 functools 模块被用户调用。
偏函数 partial 应用
函数在执行时，要带上所有必要的参数进行调用。但是，有时参数可以在函数被调用之前提前获知。这种情况下，一个函数有一个或多个参数预先就能用上，以便函数能用更少的参数进行调用。

偏函数是将所要承载的函数作为partial()函数的第一个参数，原函数的各个参数依次作为partial()函数后续的参数，除非使用关键字参数。
对于整数 100，取得对于不同数 m 的 100%m 的余数。

```
from functools import partial

def mod(n, m):
    return n % m
mod_by_100 = partial(mod, 100)    #100为mod的第一个参数

print(mod_by_100( 7 ) ) #打印 2    7作为mod的第二个参数

```

