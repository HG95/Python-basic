# Python itertools 模块

## `itertools`库

迭代器的特点是：`惰性求值（Lazy evaluation`），即只有当迭代至某个值时，它才会被计算，这个特点使得迭代器特别适合于遍历大文件或无限集合等，因为我们不用一次性将它们存储在内存中。

Python 内置的 itertools 模块包含了一系列用来产生不同类型迭代器的函数或类，这些函数的返回都是一个迭代器，我们可以通过 for 循环来遍历取值，也可以使用`next()`来取值。

`itertools` 模块提供的迭代器函数有以下几种类型：

- 无限迭代器：生成一个无限序列，比如自然数序列 1, 2, 3, 4, …；
- 有限迭代器：接收一个或多个序列（sequence）作为参数，进行组合、分组和过滤等；
- 组合生成器：序列的排列、组合，求序列的笛卡儿积等

### 无限迭代器

`itertools `模块提供了三个函数（事实上，它们是类）用于生成一个无限序列迭代器：

```python
count(firstval=0, step=1)
	
	创建一个从 firstval (默认值为 0) 开始，以 step (默认值为 1) 
	为步长的的无限整数迭代器

cycle(iterable)
	对 iterable 中的元素反复执行循环，返回迭代器

repeat(object [,times]
	反复生成 object，如果给定 times，则重复次数为 times，否则为无限

```

#### `count`

`count()` 接收两个参数，第一个参数指定开始值，默认为 0，第二个参数指定步长，默认为 1：

```python
>>> import itertools
>>>
>>> nums = itertools.count()
>>> for i in nums:
...     if i > 6:
...         break
...     print i
...
0
1
2
3
4
5
6
>>> nums = itertools.count(10, 2)    # 指定开始值和步长
>>> for i in nums:
...     if i > 20:
...         break
...     print i
...
10
12
14
16
18
20

```

#### `cycle`

`cycle() `用于对 iterable 中的元素反复执行循环：

```python
>>> import itertools
>>>
>>> cycle_strings = itertools.cycle('ABC')
>>> i = 1
>>> for string in cycle_strings:
...     if i == 10:
...         break
...     print i, string
...     i += 1
...
1 A
2 B
3 C
4 A
5 B
6 C
7 A
8 B
9 C

```

#### `repeat`

`1repeat()` 用于反复生成一个 object：

```python
>>> import itertools
>>>
>>> for item in itertools.repeat('hello world', 3):
...     print item
...
hello world
hello world
hello world
>>>
>>> for item in itertools.repeat([1, 2, 3, 4], 3):
...     print item
...
[1, 2, 3, 4]
[1, 2, 3, 4]
[1, 2, 3, 4]

```

### 有限迭代器

`itertools 模块`提供了多个函数（类），接收一个或多个迭代对象作为参数，对它们进行`组合`、`分组`和`过滤`等：

#### `chain`

```
chain(iterable1, iterable2, iterable3, ...)

chain 接收多个可迭代对象作为参数，将它们『连接』起来，作为一个新的迭代器返回。

```

```python
>>> from itertools import chain
>>>
>>> for item in chain([1, 2, 3], ['a', 'b', 'c']):
				#chain([1, 2, 3], ['a', 'b', 'c'])==[1, 2, 3, 'a', 'b', 'c']
...     print item
...
1
2
3
a
b
c

```

#### `compress`

compress 的使用形式如下：

```python
compress(data, selectors)

compress 可用于对数据进行筛选，当 selectors 的某个元素为 true 时，
则保留 data 对应位置的元素，否则去除：

```

#### `dropwhile`

```python
dropwhile(predicate, iterable)

其中，predicate 是函数，iterable 是可迭代对象。
对于 iterable 中的元素，如果 predicate(item) 为 true，则丢弃该元素，
**否则返回该项及所有后续项**。

```

```python
>>> from itertools import dropwhile
>>>
>>> list(dropwhile(lambda x: x < 5, [1, 3, 6, 2, 1]))
#返回6及6后面的item
[6, 2, 1]
>>>
>>> list(dropwhile(lambda x: x > 3, [2, 1, 6, 5, 4]))
[2, 1, 6, 5, 4]

```

#### `groupby`

`groupby` 用于对序列进行分组，它的使用形式如下：

```python
	
groupby(iterable[, keyfunc])

其中，iterable 是一个可迭代对象，keyfunc 是分组函数，用于对 iterable 的连续项进行分组，
如果不指定，则默认对 iterable 中的连续相同项进行分组，
返回一个 (key, sub-iterator) 的迭代器。

```

```python
>>> from itertools import groupby
>>>
>>> for key, value_iter in groupby('aaabbbaaccd'):
...     print key, ':', list(value_iter)
...
a : ['a', 'a', 'a']
b : ['b', 'b', 'b']
a : ['a', 'a']
c : ['c', 'c']
d : ['d']
>>>
>>> data = ['a', 'bb', 'ccc', 'dd', 'eee', 'f']
>>> for key, value_iter in groupby(data, len):    # 使用 len 函数作为分组函数
...     print key, ':', list(value_iter)
...
1 : ['a']
2 : ['bb']
3 : ['ccc']
2 : ['dd']
3 : ['eee']
1 : ['f']
>>>
>>> data = ['a', 'bb', 'cc', 'ddd', 'eee', 'f']
>>> for key, value_iter in groupby(data, len):
...     print key, ':', list(value_iter)
...
1 : ['a']
2 : ['bb', 'cc']
3 : ['ddd', 'eee']
1 : ['f']

```

#### `ifilter` , `ifilterfalse`

`ifilter` 的使用形式如下：

```python
	
ifilter(function or None, sequence)
将 iterable 中 function(item) 为 True 的元素组成一个迭代器返回，
如果 function 是 None，则返回 iterable 中所有计算为 True 的项。

```

```python
>>> from itertools import ifilter
>>>
>>> list(ifilter(lambda x: x < 6, range(10)))
[0, 1, 2, 3, 4, 5]
>>>
>>> list(ifilter(None, [0, 1, 2, 0, 3, 4]))
[1, 2, 3, 4]

```

`ifilterfalse` 的使用形式和`ifilter`类似，它将 iterable 中 function(item) 为 False 的元素组成一个迭代器返回，如果 function 是 None，则返回 iterable 中所有计算为 False 的项。

```python
>>> from itertools import ifilterfalse
>>>
>>> list(ifilterfalse(lambda x: x < 6, range(10)))
[6, 7, 8, 9]
>>>
>>> list(ifilter(None, [0, 1, 2, 0, 3, 4]))
[0, 0]

```

#### `takewhile`

takewhile 的使用形式如下：

```python
	
takewhile(predicate, iterable)

其中，predicate 是函数，iterable 是可迭代对象。
对于 iterable 中的元素，如果 predicate(item) 为 true，则保留该元素，
只要 predicate(item) 为 false，则立即停止迭代。

```

```python
>>> from itertools import takewhile
>>>
>>> list(takewhile(lambda x: x < 5, [1, 3, 6, 2, 1]))
[1, 3]
>>> list(takewhile(lambda x: x > 3, [2, 1, 6, 5, 4]))
[]

```

使用`takewhile` 替代`break 语句`
有时，我们需要在每次循环开始时，判断循环是否需要提前结束。比如下面这样：

```python
for uer in users:
    # 当第一个不合格的用户出现后，不再进行后面的处理
    if not is_qualified(user):
        break
    
    #进行处理... ...

```

对于这类需要提前中断的循环，我们可以使用 `takewhile() 函数`来简化它。`takewhile(predicate,iterable)`会在迭代 `iterable` 的过程中不断使用当前对象作为参数调用`predicate 函数`并测试返回结果，如果函数返回值为真，则生成当前对象，循环继续。否则立即中断当前循环。

```python
from itertools import takewhile

for user in takewhile(is_qualified, users):
    # 进行处理... ...

```

#### `izip` , `izip_longest`

`izip` 用于将多个可迭代对象对**应位置**的元素**作为一个元组**，将所有元组『组成』一个迭代器，并返回。它的使用形式如下：

```
	
izip(iter1, iter2, ..., iterN)
如果某个可迭代对象不再生成值，则迭代停止。

```

```python
>>> from itertools import izip
>>> 
>>> for item in izip('ABCD', 'xy'):
...     print item
...
('A', 'x')
('B', 'y')
>>> for item in izip([1, 2, 3], ['a', 'b', 'c', 'd', 'e']):
...     print item
...
(1, 'a')
(2, 'b')
(3, 'c')

```



`izip_longest`跟 `izip`类似，但迭代过程会持续到所有可迭代对象的元素都被迭代完。它的形式如下：

```
	
izip_longest(iter1, iter2, ..., iterN, [fillvalue=None])
如果有指定 fillvalue，则会用其填充缺失的值，否则为 None。

```

```python
>>> from itertools import izip_longest
>>>
>>> for item in izip_longest('ABCD', 'xy'):
...     print item
...
('A', 'x')
('B', 'y')
('C', None)
('D', None)
>>>
>>> for item in izip_longest('ABCD', 'xy', fillvalue='-'):
...     print item
...
('A', 'x')
('B', 'y')
('C', '-')
('D', '-')

```





### 组合生成器

`itertools` 模块还提供了多个组合生成器函数，用于求序列的排列、组合等：

#### `product`

`product` 用于求多个可迭代对象的笛卡尔积，它跟嵌套的 for 循环等价。它的一般使用形式如下：

```python
product(iter1, iter2, ... iterN, [repeat=1])
其中，repeat 是一个关键字参数，用于指定重复生成序列的次数，

```

```python
>>> from itertools import product
>>>
>>> for item in product('ABCD', 'xy'):
...     print item
...
('A', 'x')
('A', 'y')
('B', 'x')
('B', 'y')
('C', 'x')
('C', 'y')
('D', 'x')
('D', 'y')
>>>
>>> list(product('ab', range(3)))
[('a', 0), ('a', 1), ('a', 2), ('b', 0), ('b', 1), ('b', 2)]
>>>
>>> list(product((0,1), (0,1), (0,1)))
[(0, 0, 0), (0, 0, 1), (0, 1, 0), (0, 1, 1), (1, 0, 0), (1, 0, 1), (1, 1, 0), (1, 1, 1)]
>>>
>>> list(product('ABC', repeat=2))
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'B'), ('B', 'C'), ('C', 'A'), ('C', 'B'), ('C', 'C')]
>>>

```

使用 product 扁平化多层嵌套循环
虽然我们都知道“扁平的代码比嵌套的好”。但有时针对某类需求，似乎一定得写多层嵌套循环才行。比如下面这段：

```python
def find_twelve(num_list1, num_list2, num_list3):
    '''
    从 3 个数字列表中，寻找是否存在和为 12 的 3 个数
    '''
    for num1 in num_list1:
        for num2 in num_list2:
            for num3 in num_list3:
                if num1 + num2 + num3 == 12:
                    return num1, num2, num3

```

对于这种需要嵌套遍历多个对象的多层循环代码，我们可以使用 `product() 函数`来优化它。`product()`可以接收多个可迭代对象，然后根据它们的**笛卡尔积不断生成结果**。

```python
from itertools import product


def find_twelve_v2(num_list1, num_list2, num_list3):
    for num1, num2, num3 in product(num_list1, num_list2, num_list3):
        if num1 + num2 + num3 == 12:
            return num1, num2, num3

```

#### `permutations`

`permutations` 用于生成一个排列，它的一般使用形式如下： 

**返回的是元组列表**

```python
	
permutations(iterable[, r])
其中，r 指定生成排列的元素的长度，如果不指定，则默认为可迭代对象的元素长度。

```

```python
>>> from itertools import permutations
>>>
>>> permutations('ABC', 2)
<itertools.permutations object at 0x1074d9c50>
>>>
>>> list(permutations('ABC', 2))
[('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
>>>
>>> list(permutations('ABC'))
[('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]
>>>

```

#### `combinations`

`combinations `用于求序列的组合，它的使用形式如下：

```python
	
combinations(iterable, r)
其中，r 指定生成组合的元素的长度。

```

```python
>>> from itertools import combinations
>>>
>>> list(combinations('ABC', 2))
[('A', 'B'), ('A', 'C'), ('B', 'C')]


```

`combinations_with_replacement` 和`combinations`类似，但它生成的组合包含自身元素。

```python
>>> from itertools import combinations_with_replacement
>>>
>>> list(combinations_with_replacement('ABC', 2))
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]

```

#### `accumulate`

简单来说就是累加。

```python
from itertools import accumulate
x = accumulate(range(10))
print(list(x))
[0, 1, 3, 6, 10, 15, 21, 28, 36, 45]

```



