# Python collections模块

Python作为一个“内置电池”的编程语言，标准库里面拥有非常多好用的模块。比如今天想给大家 介绍的 `collections`就是一个非常好的例子。

## 基本介绍

Python拥有一些内置的数据类型，比如`str`, `int`, `list`, `tuple`, `dict`等， collections模块在这些内置数据类型的基础上，提供了几个额外的数据类型：

- `namedtuple()`: 生成可以使用名字来访问元素内容的tuple子类
- `deque`: 双端队列，可以快速的从另外一侧追加和推出对象
- `Counter`: 计数器，主要用来计数
- `OrderedDict`: 有序字典
- `defaultdict`: 带有默认值的字典

## `namedtuple()`

Python 中存储系列数据，比较常见的数据类型有 list，除此之外，还有 tuple 数据类型。相比与list，tuple 中的元素不可修改，在映射中可以当键使用。tuple 元组的 item 只能通过 index 访问，

collections 模块的 `namedtuple` 子类不仅可以使用 `item` 的 `index` 访问 item，还可以通过 item的 name 进行访问。可以将 `namedtuple` 理解为 c 中的 struct 结构，其首先将各个 item 命名，然后对每个 item 赋予数据。


```python
coordinate = namedtuple('Coordinate', ['x', 'y'])
co = coordinate(10,20)
print (co.x,co.y)
print (co[0],co[1])

# 也可以通过一个list来创建一个User对象，这里注意需要使用"_make"方法
co = coordinate._make([100,200])
print (co.x,co.y)
co = co._replace(x = 30)
print (co.x,co.y)


10 20
10 20
100 200
30 200

```

```python
from collections import namedtuple

websites = [
    ('Sohu', 'http://www.google.com/', u'张朝阳'),
    ('Sina', 'http://www.sina.com.cn/', u'王志东'),
    ('163', 'http://www.163.com/', u'丁磊')
]

Website = namedtuple('Websites', ['name', 'url', 'founder'])
# Website  对象名
# 参数'Websites': Websites(name='Sohu', url='http://www.google.com/', founder='张朝阳')
#               指打印显示的名字

for website in websites:
    website = Website._make(website)


    print(website)
    '''
    Websites(name='Sohu', url='http://www.google.com/', founder='张朝阳')
    Websites(name='Sina', url='http://www.sina.com.cn/', founder='王志东')
    Websites(name='163', url='http://www.163.com/', founder='丁磊')
    '''

    print(website.name, website.url, website.founder)
    '''
    Sohu http://www.google.com/ 张朝阳
    Sina http://www.sina.com.cn/ 王志东
    163 http://www.163.com/ 丁磊
    '''

```

## `deque ()`

`deque` 是一个双端队列, 如果要经常从两端 append 的数据, 选择这个数据结构就比较好了, 如果要实现随机访问,不建议用这个,请用列表.
deque 优势就是可以从两边append , appendleft 数据. 这一点list 是没有的.

**指定队列长度**

```python
from collections import deque
#  可以指定 队列的长度
mydeque = deque(maxlen=10)
print(mydeque.maxlen)  # 10

```

**从两端添加元素**

```python
# 默认从右边加入
mydeque.append(10)
mydeque.append(12)
print(mydeque)  # deque([10, 12], maxlen=10)

# 也可以从左边加入
mydeque.appendleft('a')
mydeque.appendleft('b')
mydeque.appendleft('c')
print(mydeque)  # deque(['c', 'b', 'a', 10, 12], maxlen=10)

```

**取队列元素**

```python
# 出队列,返回出队列的元素 
# 可以从左边也可以从右边 出队列 
mydeque.pop()
mydeque.popleft()

```

**查看队列元素个数**

```python
# 查看 队列里面元素个数
print(len(mydeque))

```

**统计元素的个数**

```python
# 统计元素的个数
#统计a 有几个
print(mydeque.count('a'))

```

**在某个位置`insert`一个元素**

```python
# insert(i, x)
# Insert x into the deque at position i.
d1
Out[31]: deque([10, 12, 13, 14])
d1.insert(2,'frank')
d1
Out[33]: deque([10, 12, 'frank', 13, 14])


```

翻转操作

```python
#翻转操作
# deque.reverse()

mydeque
Out[52]: deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
mydeque.reverse()
mydeque
Out[54]: deque([9, 8, 7, 6, 5, 4, 3, 2, 1, 0])


```

`remove` 移除某个元素

```python
mydeque
Out[23]: deque(['e', 'd', 'c', 'b', 'a', 10, 12])
mydeque.remove(10)
mydeque
Out[25]: deque(['e', 'd', 'c', 'b', 'a', 12])

```

清空队列元素 `clear`

```python
mydeque
Out[46]: deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
mydeque.clear
Out[47]: <function deque.clear>
mydeque.clear()
mydeque
Out[49]: deque([])


```

### `rotate` 方法

移动到最后一个，占用第一个位置，循环移动， value 是步长, rotate（value) 对队列实行旋转操作（每个元素依次向后移动value步，最后一个移动到第一个算一步）

```python
from collections import deque	

d = deque()
d.extend(['a', 'b', 'c', 'd', 'e'])
d.rotate(2)  # 指定次数，默认1次
print(d) # deque(['d', 'e', 'a', 'b', 'c'])

```

`maxlen`要说明一下, 如果指定了 `maxlen` 如果构建deque 的时候,指定了maxlen, 则可以通过 d.maxlen 来获得dueue的最大长度. 如果插入的数据大于 maxlen 则会自动删除旧的元素.删除 什么元素,取决于, 从哪边添加数据

```python
d = deque(list(range(5)),maxlen=5)
d
Out[21]: deque([0, 1, 2, 3, 4])

d.maxlen
Out[26]: 5


# 从左边添加元素, # 元素4 被挤出 队列

d.appendleft('frank')
d
Out[23]: deque(['frank', 0, 1, 2, 3])

# 从右边添加元素, 元素 'frank'  被挤出队列.
d
Out[23]: deque(['frank', 0, 1, 2, 3])
d.append('xiaoming')
d
Out[25]: deque([0, 1, 2, 3, 'xiaoming'])


```

参考： <a>https://blog.csdn.net/u010339879/article/details/80767293</a>

## `Counter()`

Counter类的目的是用来跟踪值出现的次数。它是一个无序的容器类型，以字典的键值对形式存储，其中`元素作为key`，其`计数作为value`。计数值可以是任意的Interger（包括0和负数）

### **Counter类创建的四种方法：**

```python
>>> c = Counter()  # 创建一个空的Counter类
>>> c = Counter('gallahad')  # 从一个可iterable对象（list、tuple、dict、字符串等）创建
>>> c = Counter({'a': 4, 'b': 2})  # 从一个字典对象创建
>>> c = Counter(a=4, b=2)  # 从一组键值对创建


```

### **计数值的访问与缺失的键**

当所访问的键不存在时，返回0，而不是KeyError；否则返回它的计数。

```python
>>> c = Counter("abcdefgab")
>>> c["a"]
2
>>> c["c"]
1
>>> c["h"]
0


```

### **计数器的更新（`update`和`subtract`）**

可以使用一个iterable对象或者另一个Counter对象来更新键值。

计数器的更新包括增加和减少两种。其中，增加使用`update()方法`：

```python
>>> c = Counter('which')
>>> c.update('witch')  # 使用另一个iterable对象更新
>>> c['h']
3
>>> d = Counter('watch')
>>> c.update(d)  # 使用另一个Counter对象更新
>>> c['h']
4

```

减少则使用`subtract()方法`：

```python
>>> c = Counter('which')
>>> c.subtract('witch')  # 使用另一个iterable对象更新
>>> c['h']
1
>>> d = Counter('watch')
>>> c.subtract(d)  # 使用另一个Counter对象更新
>>> c['a']
-1


```

### **键的删除**

当计数值为0时，并不意味着元素被删除，删除元素应当使用`del`

```python
>>> c = Counter("abcdcba")
>>> c
Counter({'a': 2, 'c': 2, 'b': 2, 'd': 1})
>>> c["b"] = 0
>>> c
Counter({'a': 2, 'c': 2, 'd': 1, 'b': 0})
>>> del c["a"]
>>> c
Counter({'c': 2, 'b': 2, 'd': 1})


```

### `elements()`

返回一个迭代器。元素被重复了多少次，在该迭代器中就包含多少个该元素。元素排列无确定顺序，个数小于1的元素不被包含。

```python
>>> c = Counter(a=4, b=2, c=0, d=-2)
>>> list(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']

```

### `most_common([n])`

`most_common(n)`按照 counter 的计数，**按照降序，返回前 n 项组成的list**; n 忽略时返回全部
返回一个 TopN 列表。如果 n 没有被指定，则返回所有元素。当多个元素计数值相同时，排列是无确定顺序的。

```python
>>> c = Counter('abracadabra')
>>> c.most_common()
[('a', 5), ('r', 2), ('b', 2), ('c', 1), ('d', 1)]
>>> c.most_common(3)
[('a', 5), ('r', 2), ('b', 2)]

```

### 算术和集合操作

`+`、`-`、`&`、`|`操作也可以用于Counter。其中&和|操作分别返回两个Counter对象各元素的最小值和最大值。需要注意的是，得到的Counter对象将删除小于1的元素。

```python
>>> c = Counter(a=3, b=1)
>>> d = Counter(a=1, b=2)
>>> c + d  # c[x] + d[x]
Counter({'a': 4, 'b': 3})
>>> c - d  # subtract（只保留正数计数的元素）
Counter({'a': 2})
>>> c & d  # 交集:  min(c[x], d[x])
Counter({'a': 1, 'b': 1})
>>> c | d  # 并集:  max(c[x], d[x])
Counter({'a': 3, 'b': 2})

```

其他

常见做法:

- `sum(c.values()) `                           继承自字典的.values()方法返回values的列表，再求和
- `c.clear()  `                                         继承自字典的.clear()方法，清空counter
- **`list(c) `**                                             返回key组成的list
- `set(c)`                                               返回key组成的set
- `dict(c)`                                             转化成字典
- **`c.items() `**                                         转化成(元素，计数值)组成的列表
- `Counter(dict(list_of_pairs)) `   从(元素，计数值)组成的列表转化成Counter

- `c.most_common()[:-n-1:-1]`        最小n个计数的(元素，计数值)组成的列表

- `c += Counter()`                 		      利用counter的相加来去除负值和0的值



列表元素去重

给定列表  list1 = [1,2,2,3,4,4,6,5,6,7,9]:

```python
from collections import Counter

list1 = [1,2,2,3,4,4,6,5,6,7,9]

list(Counter(list1))

# [1, 2, 3, 4, 6, 5, 7, 9]
```

**题目**

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

示例 1:

输入: [3,2,3]
输出: 3
示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2

```python
import collections
class Solution2(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return [k for k, v in collections.Counter(nums).items() if v>len(nums)//2][0]
```









## `OrderedDict()`

Python中的字典对象可以以“键：值”的方式存取数据。OrderedDict 是它的一个子类，实现了对字典对象中元素的排序。

```python
import collections
 
print('Regular dictionary:')
d={}
d['a']='A'
d['b']='B'
d['c']='C'
for k,v in d.items():
    print( k,v)
 
print( '\nOrderedDict:')
d=collections.OrderedDict()
d['a']='A'
d['b']='B'
d['c']='C'
for k,v in d.items():
    print( k,v)



Regular dictionary:
a A
c C
b B

OrderedDict:
a A
b B
c C

```

可以看到，同样是保存了ABC三个元素，但是使用OrderedDict会根据放入元素的先后顺序进行排序。由于进行了排序，所以OrderedDict对象的字典对象，如果其顺序不同那么Python也会把他们当做是两个不同的对象，比如下面的代码：

```python
import collections
 
print('Regular dictionary:')
d1={}
d1['a']='A'
d1['b']='B'
d1['c']='C'
 
d2={}
d2['c']='C'
d2['a']='A'
d2['b']='B'
 
print(d1==d2)
 
print('\nOrderedDict:')
d1=collections.OrderedDict()
d1['a']='A'
d1['b']='B'
d1['c']='C'
 
d2=collections.OrderedDict()
d2['c']='C'
d2['a']='A'
d2['b']='B'
 
print(d1==d2)


Regular dictionary:
True

OrderedDict:
False

```

### `.fromkeys()`

(指定一个列表，把列表中的值作为字典的key,生成一个字典)

```python
import collections

dic = collections.OrderedDict()
name = ['tom','lucy','sam']
print(dic.fromkeys(name))
print(dic.fromkeys(name,20))

#输出：OrderedDict([('tom', None), ('lucy', None), ('sam', None)])
#     OrderedDict([('tom', 20), ('lucy', 20), ('sam', 20)])

```

### `.items()`

(返回由“键值对组成元素“的元组列表)

```python
import collections

dic = collections.OrderedDict()
dic['k1'] = 'v1'
dic['k2'] = 'v2'
print(dic.items())

#输出：odict_items([('k1', 'v1'), ('k2', 'v2')])

```

`.keys()`

(获取字典所有的key,返回一个列表)

```python
import collections

dic = collections.OrderedDict()
dic['k1'] = 'v1'
dic['k2'] = 'v2'
print(dic.keys())

# 输出：odict_keys(['k1', 'k2'])

```

`.values()`

(获取字典所有的value，返回一个列表)

```python
import collections

dic = collections.OrderedDict()
dic['k1'] = 'v1'
dic['k2'] = 'v2'
dic['k3'] = 'v3'
print(dic.values())

# 输出：odict_values(['v1', 'v2', 'v3'])

```

## `defaultdict()`

```python
class collections.default([default_factory[, ...]])

```

返回一个类字典对象。defaultdict是内置类型dict的子类。他重写了父的一个方法并且增加了一个可以的实例变量。余下的功能与字典的一样，在这里就不写文档了。
第一个参数为default_factory属性提供初始值；default_factory的默认值为None.余下的参数被视为dict构造器的参数包括关键字参数

使用`list`作为`default_factory`,他很容易的将一个以键值形式表现的序列分组成一个字典列表

```python
>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
>>> d = defaultdict(list)
>>> for k, v in s:
...     d[k].append(v)
...
>>> d.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]

```

当每个 key 第一次被访问时，他们肯定没有在映射中：一个入口自动的被创建了使用`default_factory` 函数返回一个空的列表。然后 list.append() 操作将值放入新的列表中。当 key再次被访问时，查找工作正常（返回这个key的列表）然后 list.append() 操作为列表添加别外的值。这个技巧比等价的技艺（使用 dict.setdefault()) 更简节，更快

```python
>>> d = {}
>>> for k, v in s:
...     d.setdefault(k, []).append(v)
...
>>> d.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]

```

将`default_factory`设置成`int`使defaultdict对于**计数非常有用**

```python
>>> s = 'mississippi'
>>> d = defaultdict(int)
>>> for k in s:
...     d[k] += 1
...
>>> d.items()
[('i', 4), ('p', 2), ('s', 4), ('m', 1)]

```

当一个字每第一次被访问时，这个字母在映射中不存在，因此default_factory函数调用`int()`**提供一个默认的0作为count**。然后自增操作计算每个字母出现的次数,
`int()函数`总是返回0，因为他是常量函数的一个特殊的例子。一个更快更灵活的方式是创建一个常量函数

设置`default_factory`为`set`使 defaultdict 构建集合字典非常有用

```python
>>> s = [('red', 1), ('blue', 2), ('red', 3), ('blue', 4), ('red', 1), ('blue', 4)]
>>> d = defaultdict(set)
>>> for k, v in s:
...     d[k].add(v)
...
>>> d.items()
[('blue', set([2, 4])), ('red', set([1, 3]))]

```

