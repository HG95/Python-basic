# Python args和kwargs的用法

## `*args`

`*args`用来将参数打包成tuple给函数体调用

```python
def function(*args):
    print(args, type(args))

function(1)
#(1,) <class 'tuple'>


def function(x, y, *args):
    print(x, y, args)

function(1, 2, 3, 4, 5)
#1 2 (3, 4, 5)


```

## `**kwargs`

`**kwargs`打包关键字参数成 dict 给函数体调用

```python
def function(**kwargs):
    print( kwargs, type(kwargs))

function(a=2)
#{'a': 2} <class 'dict'>


def function(**kwargs):
    print(kwargs)

function(a=1, b=2, c=3)

#{'a': 1, 'b': 2, 'c': 3}

```

实际应用中，按照`解压序列`进行参数传递

## 打包 

打包就是将传递给函数的任意多个（也可以是零个）非关键字参数/关键字参数打包成一个元组/字典（元组只能接收非关键字参数，字典只能接收关键字参数）
元组打包的例子

```python
#!/usr/bin/env python
#coding=utf-8
def tuple_pack(a, *b):
    print(a)
    print(b)
tuple_pack(1,2,3,4,5)

1
(2,3,4,5)


```

在该例子中，`*b`这个位置可以接收任意多个（也可以是零个）非关键字参数，并将收集到的参数转换成一个元组。
字典打包的例子

```python
def dictionary_pack(a, **b):
    print(a)
    print(b)
dictionary_pack(1,one=1,two=2,three=3,four=4)

1
{'four':4，'one':2,'three':4,'two':3}
```

在该例子中，`**b` 这个位置可以接收任意多个（也可以是零个）关键字参数，并将收集到的参数转换成一个字典。
元组和字典混合的例子

```python
def tuple_dictionary_pack(*a, **b):
    print(a)
    print(b)
tuple_dictionary_pack(1,2,3,one=2,two=2)
(1,2,3)
{'one':4,'two':5}
```

## 拆解

拆解就是将传递给函数的一个列表、元组或字典拆分成独立的多个元素然后赋值给函数中的参变量（包括普通的位置参数，关键字参数，元组也即`*`非关键字参数，字典也即`**`关键字参数）。
在解字典时会有两种解法，一种使用`*`解，解出来传给函数的只有键值（.key）另一种是用`**`解，解出来的是字典的每一项。
位置变量和元组混合拆解的例子

```python
#!/usr/bin/env python
#coding=utf-8
def variable_tuple_unpack(a,b,c,*d):
    print(c)
    print(d)
ee = [1,2,3,4,5]
variable_tuple_unpack(*ee)
输出的结果是：
3
(4,5)


```

元组和字典混合的例子

```python
#!/usr/bin/env python
#coding=utf-8
def tuple_dictionary_unpack(*a,**b):
    print(a)
    print(b)
ee = (1,2,3)
ff = {'one':1,'two':2,'three':3} 
tuple_dictionary_unpack(*ee,**ff)


(1,2,3)
{'one':1,'three':3,'two':2}


```

字典的键值解成元组的例子

```python
#!/usr/bin/env python
#coding=utf-8
def tuple_dictionary(*a):
    print(a)
ff = {'one':1,'two':2,'three':3} 
tuple_dictionary(*ff)

('one','three','two')


print(*ff)
#('one', 'two', 'three')

```

