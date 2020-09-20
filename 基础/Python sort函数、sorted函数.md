# Python 字符串 sort()函数、sorted()函数

## sort 与 sorted 区别：

`sort` 是应用在 list 上的方法，`sorted `可以对所有可迭代的对象进行排序操作。
list 的 sort 方法返回的是对已经存在的列表进行操作，返回值为None，

而内建函数 `sorted` 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。


用 sort 函数对列表排序时会影响列表本身，而sorted不会。

```python
>>> a = [1,2,1,4,3,5]
>>> a.sort()
>>> a
[1, 1, 2, 3, 4, 5]
>>> a = [1,2,1,4,3,5]
>>> sorted(a)
[1, 1, 2, 3, 4, 5]
>>> a
[1, 2, 1, 4, 3, 5]

```

## `sort()`

```python
sort() 函数用于对原列表进行排序，如果指定参数，则使用比较函数指定的比较函数。
sort()方法语法：
	list.sort( key=None, reverse=False)
参数
	key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，
    		指定可迭代对象中的一个元素来进行排序。
	reverse -- 排序规则，reverse = True 降序， reverse = False 升序（默认）。
返回值
	该方法没有返回值，但是会对列表的对象进行排序。

```

```python
# 获取列表的第二个元素
def takeSecond(elem):
    return elem[1]
 
# 列表
random = [(2, 2), (3, 4), (4, 1), (1, 3)]
 
# 指定第二个元素排序
random.sort(key=takeSecond)
 
# 输出类别
print ('排序列表：', random)

```

## `sorted()`

```
sorted 语法：
	sorted(iterable, key=None, reverse=False)  
参数说明：
	iterable -- 可迭代对象。
	key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，
			指定可迭代对象中的一个元素来进行排序。
	reverse -- 排序规则，reverse = True 降序 ， reverse = False 升序（默认）。
返回值
	返回重新排序的列表。

```

sorted 的第一个参数是一个迭代器，第二个参数是用来排序的key，第三个参数的排序数序：正序还是倒序

第一个参数是迭代器

```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]

```

第二个参数是用来排序的 key

```python
>>> sorted([36, 5, -12, 9, -21], key=abs)  # 绝对值
[5, 9, -12, -21, 36]

```

key 指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序。对比原始的list和经过key=abs 处理过的 list：
list = [36, 5, -12, 9, -21]
keys = [36, 5, 12, 9, 21]
第三个参数决定正向还是反向排序：
要进行反向排序，不必改动key函数，可以传入第三个参数reverse=True：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']


```

```python
student_tuples = [('john', 'A', 15),
                  ('jane', 'B', 12),
                  ('dave', 'B', 10)]
new_tuples = sorted(student_tuples, key=lambda student: student[2])
print(student_tuples)
print(new_tuples)

```



由于这种含有 key 参数的方法很普遍，所以 python 中提供了一些方法使得访问器函数更加方便。比如operator模块中的`itemgetter()`, `attrgetter()`方法。

```python
from operator import itemgetter, attrgetter

class Student:
    def __init__(self, name, grade, age):
        self.name = name
        self.grade = grade
        self.age = age

student_objects = [Student('john', 'A', 15),
                   Student('jane', 'B', 12),
                   Student('dave', 'B', 10)]
student_tuples = [('john', 'A', 15),
                  ('jane', 'B', 12),
                  ('dave', 'B', 10) ]

result1 = sorted(student_tuples, key=itemgetter(2))  # 通过元素的第三个值排序
result2 = sorted(student_objects, key=attrgetter('age'))  # 通过对象的age属性排序
result3 = sorted(student_tuples, key=itemgetter(1,2))  # 首先通过元素的第一个值排序，然后通过第二个值排序
result4 = sorted(student_objects, key=attrgetter('grade', 'age'))  # 通过对象的grade属性排序，后通过age属性排序

```

排序是保证稳定可靠的，当排序的key对应的值相同时，会保持它们在原数据中的顺序，比sort里的第3个例子如以下代码运行结果：

```python
from operator import itemgetter
data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]
print(sorted(data, key=itemgetter(0)))

#[('blue', 1), ('blue', 2), ('red', 1), ('red', 2)]

```

###  `sorted`对字典进行排序

`sorted(iterable,key,reverse)`，sorted一共有iterable,key,reverse这三个参数。

对字典排序，可以根据`key`,`进行排序，对value排序，需要用到`key`

1. 要按`key`值对字典排序，则可以使用如下语句：

```python
dic = {'c': 10, 'a': 8, 'b': '3'}
sorted(dic.keys())

#结果：['a', 'b', 'c']

```

直接使用`sorted(d.keys())`就能按key值对字典排序，这里是按照顺序对key值排序的，如果想按照倒序排序的话，则只要将`reverse`置为true即可。

2. sorted函数按`value`值对字典排序
   要对字典的value排序则需要用到key参数，在这里主要提供一种使用lambda表达式的方法，如下：

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503113045.png"/>