# python Dataclasses

﻿原文链接：<https://medium.com/mindorks/understanding-python-dataclasses-part-2-660ecc11c9b8>
参考：<https://linux.cn/article-9974-1.html>

## 介绍

`Dataclasses` 是 Python 的类（LCTT 译注：更准确的说，它是一个模块），适用于存储数据对象。你可能会问什么是数据对象？下面是定义数据对象的一个不太详细的特性列表：

- 它们存储数据并代表某种数据类型。例如：一个数字。对于熟悉 ORM 的人来说，模型实例就是一个数据对象。它代表一种特定的实体。它包含那些定义或表示实体的属性。
- 它们可以与同一类型的其他对象进行比较。例如：一个数字可以是 `greater than`（大于）、`less than`（小于） 或` equal`（等于） 另一个数字。


<font color=#D2691E size=4>Python 3.7 提供了一个`装饰器 dataclass`，用于将类转换为 `dataclass`

你所要做的就是将类包在装饰器中：

```python
from dataclasses import dataclass

@dataclass
class A:
 ...
```

## 初始化

通常是这样：

```python
class Number:
    def __init__(self, val):
        self.val = val
>>> one = Number(1)
>>> one.val
>>> 1
```

用 `dataclass` 是这样：

```python
@dataclass
class Number:
    val:int 
>>> one = Number(1)
>>> one.val
>>> 1
```

以下是 `dataclass 装饰器`带来的变化：

1. 无需定义` __init__`，然后将值赋给 `self`，dataclass 负责处理它;
2. 们以更加易读的方式预先定义了成员属性，以及类型提示。我们现在立即能知道 `val `是` int` 类型。这无疑比一般定义类成员的方式更具可读性。

它也可以定义默认值：

```python
@dataclass
class Number:
    val:int = 0
```

## 表示

对象表示指的是对象的一个有意义的字符串表示，它在调试时非常有用。
默认的 Python 对象表示不是很直观：

```python
class Number:
    def __init__(self, val = 0):
    self.val = val
>>> a = Number(1)
>>> a
>>> <__main__.Number object at 0x7ff395b2ccc0>
```

这让我们无法知悉对象的作用，并且会导致糟糕的调试体验。

一个有意义的表示可以通过在类中定义一个` __repr__ `方法来实现。

```python
def __repr__(self):
    return self.val
```

现在我们得到这个对象有意义的表示：

```python
>>> a = Number(1)
>>> a
>>> 1
```

`dataclass `会自动添加一个 `__repr__  `函数，这样我们就不必手动实现它了。

```python
@dataclass
class Number:
    val: int = 0
```

```python
>>> a = Number(1)
>>> a
>>> Number(val = 1)

```

## 数据比较

通常，数据对象之间需要相互比较。
两个对象 a 和 b 之间的比较通常包括以下操作：

- a < b
- a > b
- a == b
- a >= b
- a <= b
  参考：<https://docs.python.org/3/reference/datamodel.html#object.__lt__>
  通常这样写：

```python
class Number:
    def __init__( self, val = 0):
       self.val = val
    def __eq__(self, other):
        return self.val == other.val
    def __lt__(self, other):
        return self.val < other.val

```

使用` dataclass`：

```python
@dataclass(order = True)
class Number:
    val: int = 0

```

我们不需要定义` __eq__ `和 `__lt__ `方法，因为当 `order = True` 被调用时，`dataclass `装饰器会自动将它们添加到我们的类定义中。

那么，它是如何做到的呢？

当你使用 dataclass 时，它会在类定义中添加函数 __eq__ 和 __lt__ 。我们已经知道这点了。那么，这些函数是怎样知道如何检查相等并进行比较呢？

生成 `__eq__ `函数的 `dataclass `类会比较两个属性构成的元组，一个由自己属性构成的，另一个由同类的其他实例的属性构成。在我们的例子中，自动生成的 `__eq__ `函数相当于：

```python
def __eq__(self, other):
    return (self.val,) == (other.val,)

```

让我们来看一个更详细的例子：

我们会编写一个` dataclass` 类 `Person `来保存 `name `和 `age`。

```python
@dataclass(order = True)
class Person:
    name: str
    age:int = 0

```

自动生成的` __eq__ `方法等同于：

```python
def __eq__(self, other):
    return (self.name, self.age) == ( other.name, other.age)

```

同样，等效的` __le__ `函数类似于：

```python
def __le__(self, other):
    return (self.name, self.age) <= (other.name, other.age)

```

当你需要对数据对象列表进行排序时，通常会出现像` __le__ `这样的函数的定义。Python 内置的 `sorted `函数依赖于比较两个对象。

```python
>>> import random
>>> a = [Number(random.randint(1,10)) for _ in range(10)] #generate list of random numbers
>>> a
>>> [Number(val=2), Number(val=7), Number(val=6), Number(val=5), Number(val=10), Number(val=9), Number(val=1), Number(val=10), Number(val=1), Number(val=7)]
>>> sorted_a = sorted(a) #Sort Numbers in ascending order
>>> [Number(val=1), Number(val=1), Number(val=2), Number(val=5), Number(val=6), Number(val=7), Number(val=7), Number(val=9), Number(val=10), Number(val=10)]
>>> reverse_sorted_a = sorted(a, reverse = True) #Sort Numbers in descending order 
>>> reverse_sorted_a
>>> [Number(val=10), Number(val=10), Number(val=9), Number(val=7), Number(val=7), Number(val=6), Number(val=5), Number(val=2), Number(val=1), Number(val=1)]

```

## `dataclass `作为一个可调用的装饰器

定义所有的` dunder`（LCTT 译注：这是指双下划线方法，即魔法方法）方法并不总是值得的。你的用例可能只包括存储值和检查相等性。因此，你只需定义 `__init__` 和 `__eq__ `方法。如果我们可以告诉装饰器不生成其他方法，那么它会减少一些开销，并且我们将在数据对象上有正确的操作。

幸运的是，这可以通过将 `dataclass `装饰器作为可调用对象来实现。

从[官方文档](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass)来看，装饰器可以用作具有如下参数的可调用对象：

```python
@dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False)
class C:
 …
```

- `init`：默认将生成 `__init__ `方法。如果传入 False，那么该类将不会有 `__init__ `方法。
- `repr`：`__repr__ `方法默认生成。如果传入 False，那么该类将不会有 `__repr__ `方法。
- `eq`：默认将生成` __eq__ `方法。如果传入 False，那么` __eq__ `方法将不会被 dataclass 添加，但默认为 object.__eq__。
- `order`：默认将生成` __gt__`、`__ge__`、`__lt__`、`__le__ `方法。如果传入 False，则省略它们。

## `Frozen`（不可变） 实例

`Frozen` 实例是在初始化对象后无法修改其属性的对象。

以下是我们期望不可变对象能够做到的：

```python
>>> a = Number(10) #Assuming Number class is immutable
>>> a.val = 10 # Raises Error
```

有了 `dataclass`，就可以通过使用 dataclass 装饰器作为可调用对象配合参数 `frozen=True` 来定义一个 `frozen 对象`。

当实例化一个 `frozen `对象时，任何企图修改对象属性的行为都会引发 `FrozenInstanceError`。

```python
@dataclass(frozen = True)
class Number:
    val: int = 0
>>> a = Number(1)
>>> a.val
>>> 1
>>> a.val = 2
>>> Traceback (most recent call last):
 File “<stdin>”, line 1, in <module>
 File “<string>”, line 3, in __setattr__
dataclasses.FrozenInstanceError: cannot assign to field ‘val’
```

因此，一个` frozen` 实例是一种很好方式来存储：

- 常数
- 设置

这些通常不会在应用程序的生命周期内发生变化，任何企图修改它们的行为都应该被禁止。

## 后期初始化处理

有了 `dataclass`，需要定义一个 `__init__ `方法来将变量赋给 `self `这种初始化操作已经得到了处理。但是我们失去了在变量被赋值之后立即需要的函数调用或处理的灵活性。

让我们来讨论一个用例，在这个用例中，我们定义一个 `Float 类`来包含浮点数，然后在初始化之后立即计算整数和小数部分。

```python
在这里插入代码片
```

```python
import math
class Float:
    def __init__(self, val = 0):
        self.val = val
        self.process()
    def process(self):
        self.decimal, self.integer = math.modf(self.val)
>>> a = Float( 2.2)
>>> a.decimal
>>> 0.2000
>>> a.integer
>>> 2.0
```

幸运的是，使用 `__post_init__`  方法已经能够处理后期初始化操作。
生成的 `__init__`  方法在返回之前调用 `__post_init__ `返回。因此，可以在函数中进行任何处理。

```python
import math
@dataclass
class FloatNumber:
    val: float = 0.0
    def __post_init__(self):
        self.decimal, self.integer = math.modf(self.val)
>>> a = FloatNumber(2.2)
>>> a.val
>>> 2.2
>>> a.integer
>>> 2.0
>>> a.decimal
>>> 0.2
```

## 继承

`Dataclasses` 支持继承，就像普通的 Python 类一样。
因此，父类中定义的属性将在子类中可用。

```python
@dataclass
class Person:
    age: int = 0
    name: str
@dataclass
class Student(Person):
    grade: int
>>> s = Student(20, "John Doe", 12)
>>> s.age
>>> 20
>>> s.name
>>> "John Doe"
>>> s.grade
>>> 12
```

请注意，`Student `的参数是在类中定义的字段的顺序。

继承过程中 `__post_init__ `的行为是怎样的？

由于` __post_init__ `只是另一个函数，因此必须以传统方式调用它：

```python
@dataclass
class A:
    a: int
    def __post_init__(self):
        print("A")
@dataclass
class B(A):
    b: int
    def __post_init__(self):
        print("B")
>>> a = B(1,2)
>>> B
```

在上面的例子中，只有 B 的 `__post_init__ `被调用，那么我们如何调用 A 的 `__post_init__` 呢？

```python
@dataclass
class B(A):
    b: int
    def __post_init__(self):
        super().__post_init__() # 调用 A 的 post init
        print("B")
>>> a = B(1,2)
>>> A
    B
```

## ` Dataclass fields `

我们已经知道 Dataclasses 会生成他们自身的`__init__`方法。它同时把初始化的值赋给这些字段。在上一面定义的内容：

• 变量名

• 数据类型

这些内容仅给我们有限的 dataclass 字段使用范围。让我们讨论一下这些局限性，以及它们如何通过 `dataclass.field` 被解决。

复合初始化

考虑以下情形：你想要初始化一个变量为列表。你如何实现它呢?一种简单的方式是使用`__post_init__`方法。

```python
import random

from typing import List

def get_random_marks():
	return [random.randint(1,10) for _ in range(5)]

@dataclass
class Student:
	marks: List[int]
	
	def __post_init__(self):
    	self.marks = get_random_marks() #Assign random speeds


>>> a = Student()
>>> a.marks
>>> [1,4,2,6,9]
```

数据类 Student 产生了一个名为 marks 的列表。我们不传递 marks 的值，而是使用`__post_init__`方法初始化。这是我们定义的单一属性。此外，我们必须在`__post_init__`里调用 `get_random_marks 函数`。这些工作是额外的。

辛运的是，Python 为我们提供了一个解决方案。我们可以使用 `dataclasses.field `来定制化 dataclass 字段的行为以及它们在 dataclass 的影响。

仍然是上述的使用情形，让我们从`__post_init__`里去除` get_random_marks `的调用。以下是使用` dataclasses.field `的情形：

```python
from dataclasses import field

@dataclass
class Student:
	marks: List[int] = field(default_factory = get_random_marks)
	
>>> s = Student()
>>> s.marks
>>> [1,4,2,6,9]
```

`dataclasses.field `接受了一个名为 `default_factory `的参数，它的作用是：如果在创建对象时没有赋值，则使用该方法初始化该字段。
`default_factory `必须是一个可以调用的无参数方法(通常为一个函数)。

这样我们就可以使用复合形式初始化字段。现在，让我们考虑另一个使用场景。
dataclass 能够自动生成`< `, `=`, `>`, `<=`和`>=`这些比较方法。但是这些比较方法的一个缺陷是，它们使用类中的所有字段进行比较，而这种情况往往不常见。更经常地，这种比较方法会给我们使用 dataclasses 造成麻烦。

考虑以下的使用情形：你有一个数据类用于存放用户的信息。现在，它可能存在以下字段：
• 姓名
• 年龄
• 身高
• 体重

你仅想比较用户对象的年龄、身高和体重。你不想比较姓名。这是后端开发者经常会遇到的使用情景。

```python
@dataclass(order = True)
class User:
	name: str
	age: int
	height: float
	weight: float
```

自动生成的比较方法会比较一下的元组：

```python
(self.name, self.age, self.height, self.weight)
```

这将会破坏我们的意图。我们不想让姓名(name)用于比较。那么，如何使用 `dataclasses.field` 来实现我们的想法呢?

```python
@dataclass(order = True)
class User:
	name:str = field(compare = False) # compare = False tells the dataclass to not use name for comparison methods
	age: int
	weight: float
	height: float
		

>>> user_1 = User("John Doe", 23, 70, 1.70)
>>> user_2 = User("Adam", 24, 65, 1.60)

>>> user_1 < user_2
>>> True
```

默认情况下，所用的字段都用于比较，因此我们仅仅需要指定哪些字段用于比较，而实现方法是直接把不需要的字段定义为` filed(compare=False)`。

一个更为简单的应用情形也可以被讨论。让我们定义一个数据类，它被用来存储一个数字激起字符串表示。我们想让比较仅仅发生在该数字的值，而不是他的字符串表示。

```python
@dataclass(order = True)
class Number:
	string: str
	val: int
	
>>> a = Number("one",1)
>>> b = Number("eight", 8)
>>> b > a # Compares as ("eight",8) > ("one",1)
>>> False

#Now we shall only compare using the Number.val

@dataclass(order = True)
class Number:
	string: str: = field(compare = False) #Do not use Number.string for comparison
	val: int
	
>>> a = Number("one", 1)
>>> b = Number("eight", 8)
>>> b > a # Compares (8,) > (1,)
>>> True
```

现在，我们有更大的自由来控制 dataclasses 的行为。

使用全部字段进行数据表示
自动生成的__repr__方法使用所有的字段用于表示。当然，这也不是大多数情形下的理想选择，尤其是当你的数据类有大量的字段时。单个对象的表示会变得异常臃肿，对调试来说也不利。

```python
@dataclass(order = True)
class User:
	name: str = field(compare = False)
	age: int
	height: float
	weight: float
	city: str = field(compare = False)
	country: str = field(compare = False)
	
>>> a = User("John Doe", 24, 1.7, 70, "Massachusetts" ,"United States of America")
>>> a
>>> User(name='John Doe', age=24, height=1.7, weight=70, city='Massachusetts', country='United States of America')  #Debugging Nightmare coming through
```

想象一下在你的日志里看到这样的表示吧，然后还要写一个正则表达式来搜索它。

当然，我们也能够个性化这种行为。考虑一个类似的使用场景，也许最合适的用于表示的属性是姓名(name)。那么对`__repr__`，我们仅使用它：

```python
@dataclass(order=True)
class User:
	name: str = field(compare=False)
	age: int = field(repr=False) # This tells the dataclass to not show age in the representation
	height:float = field(repr=False)
	weight:float = field(repr=False)
	city:str = field(repr=False, compare=False) #Do not use city for representation and comparison
	country:str = field(repr=False, compare=False)
	
>>> a = User("John Doe", 24, 1.7, 70, "Massachusetts", "United States of America")
>>> b = User("Adam", 24, 1.6, 65, "San Jose", "United States of America")

>>> a
>>> User(name='John Doe')
>>> b
>>> User(name='Adam')
>>> b > a #Compares (24, 1.7, 70) > (23, 1.6, 65)
>>> True
```

这样看起来就很棒了。调试很方便，比较也有意义!

<font color=#D2691E size=4>从初始化中省略字段

目前为止我们看到的所有例子，都有一个共同特点——即我们需要为所有被声明的字段传递值，除了有默认值之外。在那种情形下(指有默认值的情况下)，我们可以选择传递值，也可以不传递。

```python
@dataclass
class Number:
  string: str
  val:int = 0
  
>>> a = Number("Zero") #Not passing value of the field with default value
>>> a
>>> Number(string='Zero', val=0)

>>> b = Number("One",1) #Passing the default value of the field which has default declared
>>> b
>>> Number(string='One', val=1)
```

但是，还有一种情形：我们可能不想在初始化时设定某个字段的值。这也是一种常见的使用场景。也许你在追踪一个对象的状态，并且希望它在初始化时一直被设为 False。更一般地，这个值在初始化时不能够被传递。

```python
class User:
	def __init__(self, email = None):
		self.email = email
		self.verified = False 
		#This field is set during initialization, but its value cannot be set manually while creating object
```

那么，我们如何实现上述想法呢?以下是具体内容：

```python
@dataclass
class User:
  email: str = field(repr = True)
  verified: bool = field(repr = False, init = False, default = False)
	#Omit verified from representation as well as __init__
	
>>> a = User("a@test.com")
>>> a
>>> User(email='a@test.com')
>>> a.verified
>>> False

>>> b = User("b@test.com", True) #Let us try to pass the value of verified
>>> Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: __init__() takes 2 positional arguments but 3 were given
```