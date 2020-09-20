# python slots

﻿## `__slots__`

-  `__slots__`:是一个`类变量`,变量值可以是**列表,元祖,或者可迭代对象**,也可以是一个字符串(意味着所有实例只有一个数据属性)
-  引子: 使用点来访问属性本质就是在访问类或者对象的`__dict__属性字典`(类的字典是共享的,而每个实例的是独立的)
-  为何使用`__slots__`: 字典会占用大量内存, ==如果你有一个属性很少的类,但是有很多实例,为了节省内存可以使用`__slots__`取代实例的`__dict__`==
   当你定义`__slots__`后, `__slots__`就会为实例使用一种更加紧凑的内部表示。实例通过一个很小的固定大小的数组来构建, 而不是为每个实例定义一个字典, 这跟元组或列表很类似。在`__slots__`中列出的属性名在内部被映射到这个数组的指定小标上。使用`__slots__`一个不好的地方就是我们**不能再给实例添加新的属性了,只能使用在`__slots__`中定义的那些属性名**。
-  注意事项: `__slots__` 的很多特性都依赖于普通的基于字典的实现。另外,定义了`__slots__`后的类不再 支持一些普通类特性了,比如多继承。大多数情况下,你应该只在那些经常被使用到 的用作数据结构的类上定义`__slots__` 比如在程序中需要创建某个类的几百万个实例对象 。
-  关于`__slots__`的一个常见误区是它可以作为一个封装工具来防止用户给实例增加新的属性。尽管使用`__slots__`可以达到这样的目的,但是这个并不是它的初衷。更多的是用来作为一个**内存优化工具**。



## `slots`定义后  对象不再具有`__dict__`字典属性

```python
class Foo:
    __slots__='x'  #只定义一个


f1=Foo()
f1.x=1
f1.y=2  #报错  实际的过程 --->setattr-->f1.__dic__['y']=2
print(f1.__slots__)  #f1不再有__dict__

class Bar:
    __slots__=['x','y']
    
n=Bar()
n.x,n.y=1,2
n.z=3#报错
```

```python
class Foo:
    __slots__=['name','age']  #{'name':None,'age':None}

f1=Foo()
f1.name='alex'
f1.age=18
print(f1.__slots__)   #输出：['name', 'age']

print(f1.__dict__)    
#输出：
#    print(f1.__dict__)
#AttributeError: 'Foo' object has no attribute '__dict__'
# slots定义后  对象不再具有字典属性

f2=Foo()
f2.name='egon'
f2.age=19
print(f2.__slots__)


print(Foo.__dict__)
#f1与f2都没有属性字典__dict__了,统一归__slots__管,节省内存
#打印：
{'__module__': '__main__', '__slots__': ['name', 'age'],
 'age': <member 'age' of 'Foo' objects>, 
 'name': <member 'name' of 'Foo' objects>, '__doc__': None}
```



## slots使用

为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性：
比如，只允许对Student实例添加name和age属性。

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称

s = Student() # 创建新的实例
s.name = 'Michael' # 绑定属性'name'
s.age = 25 # 绑定属性'age'
s.score = 99 # 绑定属性'score'   会报错  AttributeError: 'Student' object has no attribute 'score'
```

Python 是动态语言，允许我们动态的增加属性和方法

```python
class Student(object):
    pass
    
s = Student()

s.name = "LiLei"

print(s.name)

>>> LiLei

```

同样也有办法限制属性的动态绑定

```python
class Teacher(object):
    # 用tuple定义允许绑定的属性名称，但是此限制对子类不起作用
    __slots__ = ('name', 'age') 

```

如上所示可以规定 class Teacher 只可以绑定 name 和 age 两个属性

```python
t = Teacher()

# 此处不能再添加属性
t.height = 1

>>> AttributeError: 'Teacher' object has no attribute 'height'

```

## 