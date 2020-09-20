# python 属性property

﻿## `property`

python的@property是python的一种装饰器，是用来修饰方法的。





在我们定义数据库字段类的时候,往往需要对其中的类属性做一些限制,一般用 get 和 set 方法来写, 那在 python 中,我们该怎么做能够少写代码,又能优雅的实现想要的限制,减少错误的发生呢,这时候就需要我们的`@property`

property属性的定义和调用要注意一下几点：

- 定义时，在实例方法上添加 `@property `装饰器；并且仅有一个self参数
- 调用时，无需括号
  一个静态属性property本质就是实现了`get`，`set`，`delete`三种方法

 由于python进行属性的定义时，没办法设置私有属性，因此要通过@property的方法来进行设置。这样可以隐藏属性名，让用户进行使用的时候无法随意修改。

## 什么是property属性

一种用起来像是使用的实例属性一样的特殊属性，可以对应于某个方法

```python
class Foo:
    def func(self):
        pass

    # 定义property属性
    @property
    def prop(self):
        pass

# ############### 调用 ###############
foo_obj = Foo()
foo_obj.func()  # 调用实例方法
foo_obj.prop  # 调用property属性
```

```python
class Goods(object):

        @property
        def size(self):
                return 100 
g = Goods()
print(g.size)
```

`property`属性的定义和调用要注意一下几点：

- 定义时，在实例方法的基础上添加 @property 装饰器；并且仅有一个self参数
- 调用时，无需括号

>对于京东商城中显示电脑主机的列表页面，每次请求不可能把数据库中的所有内容都显示到页面上，而是通过分页的功能局部显示，所以在向数据库中请求数据时就要显示的指定获取从第m条到第n条的所有数据 这个分页的功能包括：
>根据用户请求的当前页和总数据条数计算出 m 和 n
>根据m 和 n 去数据库中请求数据

```python
# ############### 定义 ###############
class Pager:
    def __init__(self, current_page):
        # 用户当前请求的页码（第一页、第二页...）
        self.current_page = current_page
        # 每页默认显示10条数据
        self.per_items = 10 

    @property
    def start(self):
        val = (self.current_page - 1) * self.per_items
        return val

    @property
    def end(self):
        val = self.current_page * self.per_items
        return val

# ############### 调用 ###############
p = Pager(1)
p.start  # 就是起始值，即：m
p.end  # 就是结束值，即：n
```

从上述可见：

Python的property属性的功能是：property属性内部进行一系列的逻辑计算，最终将计算结果返回。

## property属性的有两种方式

- 装饰器 即：在方法上应用装饰器
- 类属性 即：在类中定义值为property对象的类属性
  新式类，具有三种@property装饰器：

```python
#coding=utf-8
# ############### 定义 ###############
class Goods:
    """python3中默认继承object类
        以python2、3执行此程序的结果不同，因为只有在python3中才有@xxx.setter  @xxx.deleter
    """
    @property
    def price(self):
        print('@property')

    @price.setter
    def price(self, value):
        print('@price.setter')

    @price.deleter
    def price(self):
        print('@price.deleter')

# ############### 调用 ###############
obj = Goods()
obj.price          # 自动执行 @property 修饰的 price 方法，并获取方法的返回值
obj.price = 123    # 自动执行 @price.setter 修饰的 price 方法，并将  123 赋值给方法的参数
del obj.price      # 自动执行 @price.deleter 修饰的 price 方法
```

新式类中的属性有三种访问方式，并分别对应了三个被`@property`、`@方法名.setter`、`@方法名.deleter` 修饰的方法

- 被 `@property` 装饰的方法是**获取属性值的方法**，被装饰方法的名字会被用做 `属性名`。
- 被 `@属性名.setter` 装饰的方法是**设置属性值的方法**。
- 被 `@属性名.deleter` 装饰的方法是**删除属性值的方法**。

由于新式类中具有三种访问方式，我们可以根据它们几个属性的访问特点，分别将三个方法定义为对同一个属性：获取、修改、删除

```python
class Goods(object):

    def __init__(self):
        # 原价
        self.original_price = 100
        # 折扣
        self.discount = 0.8

    @property
    def price(self):
        # 实际价格 = 原价 * 折扣
        new_price = self.original_price * self.discount
        return new_price

    @price.setter
    def price(self, value):
        self.original_price = value

    @price.deleter
    def price(self):
        del self.original_price

obj = Goods()
obj.price         # 获取商品价格
obj.price = 200   # 修改商品原价
del obj.price     # 删除商品原价
```

使用`property`取代 `getter` 和`setter` 方法

```python
class Money(object):
    def __init__(self):
        self.__money = 0

    # 使用装饰器对money进行装饰，那么会自动添加一个叫money的属性，当调用获取money的值时，
    # 调用装饰的方法
    @property
    def money(self):
        return self.__money

    # 使用装饰器对money进行装饰，当对money设置值时，调用装饰的方法
    @money.setter
    def money(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error:不是整型数字")

a = Money()
a.money = 100
print(a.money)
```

Case 1 

```python
class Goods:
    def __init__(self):
        # 原价
        self.original_price = 100
        # 折扣
        self.discount = 0.8

    @property
    def price(self):
        # 实际价格 = 原价 * 折扣
        new_price = self.original_price * self.discount
        return new_price

    @price.setter
    def price(self, value):
        self.original_price = value

    @price.deleter
    def price(self):
        del self.original_price


obj = Goods()
obj.price         # 获取商品价格
obj.price = 200   # 修改商品原价
print(obj.price)
del obj.price     # 删除商品原价
```



Case 2

```python
#实现类型检测功能

#第一关：
class People:
    def __init__(self,name):
        self.name=name

    @property
    def name(self):
        return self.name

# p1=People('alex') 
# property自动实现了set和get方法属于数据描述符,比实例属性优先级高,
# 所以你这面写会触发property内置的set,抛出异常


#第二关：修订版

class People:
    def __init__(self,name):
        self.name=name   #实例化就触发property

    @property
    def name(self):
        # return self.name #无限递归
        print('get------>')
        return self.DouNiWan

    @name.setter
    def name(self,value):
        print('set------>')
        self.DouNiWan=value

    @name.deleter
    def name(self):
        print('delete------>')
        del self.DouNiWan

p1=People('alex') #self.name实际是存放到self.DouNiWan里
#set------>
print(p1.name)		
#get------>
#alex
print(p1.name)
#get------>
#alex

print(p1.__dict__)
#{'DouNiWan': 'alex'}

p1.name='egon'
print(p1.__dict__)

del p1.name
#set------>
print(p1.__dict__)
#{'DouNiWan': 'egon'}

#第三关:加上类型检查
class People:
    def __init__(self,name):
        self.name=name #实例化就触发property

    @property
    def name(self):
        # return self.name #无限递归
        print('get------>')
        return self.DouNiWan

    @name.setter
    def name(self,value):
        print('set------>')
        if not isinstance(value,str):
            raise TypeError('必须是字符串类型')
        self.DouNiWan=value

    @name.deleter
    def name(self):
        print('delete------>')
        del self.DouNiWan

p1=People('alex') #self.name实际是存放到self.DouNiWan里
p1.name=1

```

```python
class People(object):
    '''
        给name属性赋值，值必须是字符串，否则抛出异常
        给age属性赋值，值必须是整数，否则抛出异常
    '''
    def __init__(self,name,age):
        #执行@name.setter装饰的name函数，在函数中给_name属性赋值
        self.name = name
        self.age = age
    #获取name的属性值
    @property
    def name(self):
        #返回属性值时，也可以添加一些额外的功能
        a = self._name
        #把姓氏换成*
        a = a.replace(a[0],'*')
        #返回name属性值
        return a
    #设置name属性的值
    @name.setter
    def name(self,value):
        #在赋值之前添加判断
        if not isinstance(value,str):
            # 抛出异常
            raise TypeError('People object.name,name must be a str!')
        #设置name的属性值
        self._name = value
        #还可以继续添加其他功能
 
    # 删除name属性
    @name.deleter
    def name(self):
        #添加一些额外的功能
        if not hasattr(self,'_name'):
            raise AttributeError('People object has no attribute "_name"')
        #删除_name属性
        del self._name
     #获取age属性的值
    @property
    def age(self):
        if self._age >=100:
            print('老妖精')
        elif 50<self._age<100:
            print('老年人')
        elif 18<self._age<=50:
            print('成年人')
        else:
            print('小屁孩')
        return self._age
    @age.setter
    def age(self,value):
        if not isinstance(value,int):
            # 抛出异常
            raise TypeError('People object.age,age must be a int value!!!!')
        self._age = value
    @age.deleter
    def age(self):
        if not hasattr(self,'_age'):
            raise AttributeError('People object has no attribute "_age"!!!')
        del self._age
p1 = People('张三',23)
# name的属性值 必须是一个字符串。否则抛出异常
p1.name = '小星星'
print(type(p1.name))
print(p1.name)
# 删除对象的属性
del p1.name
 
p1.age = 18
print(p1.age)
del p1.age

```

## 