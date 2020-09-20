# Python 生成器和生成器函数

## [列表生成器](https://blog.csdn.net/HHG20171226/article/details/102632305)

现在有个需求，看列表 [0，1，2，3，4，5，6，7，8，9]，要求你把列表里面的每个值加1，你怎么实现呢？

```python
info = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
a = map(lambda x:x+1,info)
print(a)
for i in a:
    print(i)


# 二
info = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
a = [i+1 for i in range(10)]
print(a)

```

## 生成器

### 什么是生成器？

通过列表生成式，我们可以直接创建一个列表，但是，受到内存限制，列表容量肯定是有限的，而且创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的 list，从而节省大量的空间，在Python中，这种一边循环一边计算的机制，称为`生成器`：generator

生成器是一个特殊的程序，可以被用作控制循环的迭代行为，python 中生成器是迭代器的一种，使用 `yield` 返回值函数，每次调用`yield`会暂停，而可以使用 next()函数和 send() 函数恢复生成器。

生成器类似于返回值为数组的一个函数，这个函数可以接受参数，可以被调用，但是，不同于一般的函数会一次性返回包括了所有数值的数组，生成器一次只能产生一个值，这样消耗的内存数量将大大减小，而且允许调用函数可以很快的处理前几个返回值，因此生成器看起来像是一个函数，但是表现得却像是迭代器

## python中的生成器

要创建一个 generator，有很多种方法，第一种方法很简单，只有把一个列表生成式的[]中括号改为（）小括号，就创建一个generator

```python
#列表生成式
lis = [x*x for x in range(10)]
print(lis)
#生成器
generator_ex = (x*x for x in range(10))
print(generator_ex)
 
结果：
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
<generator object <genexpr> at 0x000002A4CBF9EBA0>

```

那么创建list和generator_ex，的区别是什么呢？从表面看就是[ ]和（）,但是结果却不一样，一个打印出来是列表（因为是列表生成式），而第二个打印出来却是<generator object at 0x000002A4CBF9EBA0>，那么如何打印出来generator_ex的每一个元素呢？

### 遍历生成器内容

如果要一个个打印出来，可以通过next（）函数获得generator的下一个返回值：

```python
#生成器
generator_ex = (x*x for x in range(10))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
print(next(generator_ex))
结果：
0
1
4
9
16
25
36
49
64
81
Traceback (most recent call last):
 
  File "列表生成式.py", line 42, in <module>
 
    print(next(generator_ex))
 
StopIteration

```

大家可以看到，generator保存的是算法，每次调用next(generaotr_ex)就计算出他的下一个元素的值，直到计算出最后一个元素，没有更多的元素时，抛出StopIteration的错误，而且上面这样不断调用是一个不好的习惯，**正确的方法是使用`for循环`**，因为generator也是可迭代对象：

```python
#生成器
generator_ex = (x*x for x in range(10))
for i in generator_ex:
    print(i)
     
结果：
0
1
4
9
16
25
36
49
64
81

```

所以我们创建一个generator后，基本上永远不会调用next()，而是通过for循环来迭代，并且不需要关心StopIteration的错误，generator非常强大，如果推算的算法比较复杂，用类似列表生成式的for循环无法实现的时候，还可以用函数来实现。

**python提供了两种基本的方式**

- 生成器函数：也是用def定义的，利用关键字yield一次性返回一个结果，阻塞，重新开始

- 生成器表达式：返回一个对象，这个对象只有在需要的时候才产生结果

## 生成器函数

为什么叫生成器函数？因为它随着时间的推移生成了一个数值队列。一般的函数在执行完毕之后会返回一个值然后退出，但是生成器函数会自动挂起，然后重新拾起急需执行，他会利用yield关键字挂起函数，给调用者返回一个值，同时保留了当前的足够多的状态，可以使函数继续执行，生成器和迭代协议是密切相关的，迭代器都有一个`__next__()`成员方法，这个方法要么返回迭代的下一项，要引起异常结束迭代。

```
# 函数有了yield之后，函数名+（）就变成了生成器
# return在生成器中代表生成器的中止，直接报错
# next的作用是唤醒并继续执行
# send的作用是唤醒并继续执行，发送一个信息到生成器内部
'''生成器'''
 
def create_counter(n):
    print("create_counter")
    while True:
        yield n
        print("increment n")
        n +=1
 
gen = create_counter(2)
print(gen)
print(next(gen))
print(next(gen))
 
结果：
<generator object create_counter at 0x0000023A1694A938>

create_counter
2

increment n
3
Process finished with exit code 0

```

```python

#著名的斐波拉契数列（Fibonacci）:除第一个和第二个数外，任意一个数都可由前两个数相加得到
#1.举例：1, 1, 2, 3, 5, 8, 13, 21, 34, ...使用函数实现打印数列的任意前n项。
 
def fib(times): #times表示打印斐波拉契数列的前times位。
    n = 0
    a,b = 0,1
    while n<times:
        print(b)
        a,b = b,a+b
        n+=1
    return 'done'
 
fib(10)  #前10位：1 1 2 3 5 8 13 21 34 55
 
#2.将print(b)换成yield b,则函数会变成generator生成器。
#yield b功能是：每次执行到有yield的时候，会返回yield后面b的值给函数并且函数会暂停，直到下次调用或迭代终止；
def fib(times): #times表示打印斐波拉契数列的前times位。
    n = 0
    a,b = 0,1
    while n<times:
        yield b  
        a,b = b,a+b
        n+=1
    return 'done'
 
print(fib(10))  #<generator object fib at 0x000001659333A3B8>
 
3.对生成器进行迭代遍历元素
方法1：使用for循环
for x in fib(6):
    print(x)
''''结果如下，发现如何生成器是函数的话，使用for遍历，无法获取函数的返回值。
1
1
2
3
5
8
'''
方法2:使用next()函数来遍历迭代，可以获取生成器函数的返回值。同理也可以使用自带的__next__()函数，效果一样
f = fib(6)
while True:
    try:  #因为不停调用next会报异常，所以要捕捉处理异常。
        x = next(f)  #注意这里不能直接写next(fib(6)),否则每次都是重复调用1
        print(x)
    except StopIteration as e:
        print("生成器返回值:%s"%e.value)
        break
'''结果如下：
1
1
2
3
5
8
生成器返回值:done
'''


```

## 生成器表达式

生成器表达式来源于迭代和列表解析的组合，生成器和列表解析类似，但是它使用尖括号而不是方括号

```python
>>> # 列表解析生成列表
>>> [ x ** 3 for x in range(5)]
[0, 1, 8, 27, 64]
>>>
>>> # 生成器表达式
>>> (x ** 3 for x in range(5))
<generator object <genexpr> at 0x000000000315F678>
>>> # 两者之间转换
>>> list(x ** 3 for x in range(5))
[0, 1, 8, 27, 64]

```

> 一个迭代既可以被写成生成器函数，也可以被写成生成器表达式，均支持自动和手动迭代。而且这些生成器只支持一个active迭代，也就是说生成器的迭代器就是生成器本身。
> 　迭代器包含有next方法的实现，在正确的范围内返回期待的数据以及超出范围后能够抛出StopIteration的错误停止迭代。

可以直接作用于for循环的数据类型有以下几种：

一类是集合数据类型，如list,tuple,dict,set,str等

一类是generator，包括生成器和带yield的generator function

这些可以直接作用于for 循环的对象统称为可迭代对象：Iterable

可以使用isinstance()判断一个对象是否为可Iterable对象

```python
>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False

```

而生成器不但可以作用于for循环，还可以被next()函数不断调用并返回下一个值，直到最后抛出StopIteration错误表示无法继续返回下一个值了。

一个实现了`iter方法`的对象是可迭代的，一个实现`next方法`并且是可迭代的对象是迭代器。
可以被`next()函数`调用并不断返回下一个值的对象称为`迭代器：Iterator`。
所以一个实现了`iter方法`和`next方法`的对象就是迭代器。

**生成器都是Iterator对象，但`list`、`dict`、`str`虽然是Iterable（可迭代对象），却不是Iterator（迭代器）**。

**把`list`、`dict`、`str`等Iterable变成Iterator可以使用iter()函数：**

```python
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True

```

这是因为Python的Iterator对象表示的是一个**数据流**，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。

Iterator甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的。

**判断下列数据类型是可迭代对象or迭代器**

```python
s='hello'
l=[1,2,3,4]
t=(1,2,3)
d={'a':1}
set={1,2,3}
f=open('a.txt')

s='hello'     #字符串是可迭代对象，但不是迭代器
l=[1,2,3,4]     #列表是可迭代对象，但不是迭代器
t=(1,2,3)       #元组是可迭代对象，但不是迭代器
d={'a':1}        #字典是可迭代对象，但不是迭代器
set={1,2,3}     #集合是可迭代对象，但不是迭代器
# *************************************
f=open('test.txt') #文件是可迭代对象，是迭代器
 
#如何判断是可迭代对象，只有__iter__方法，执行该方法得到的迭代器对象。
# 及可迭代对象通过__iter__转成迭代器对象
from collections import Iterator  #迭代器
from collections import Iterable  #可迭代对象
 
print(isinstance(s,Iterator))     #判断是不是迭代器
print(isinstance(s,Iterable))       #判断是不是可迭代对象
 
#把可迭代对象转换为迭代器
print(isinstance(iter(s),Iterator))

```

注意：文件的判断

```python
f = open('housing.csv')
from collections import Iterator
from collections import Iterable
 
print(isinstance(f,Iterator))
print(isinstance(f,Iterable))
 
True
True

```

**结论：文件是可迭代对象，也是迭代器**

小结：

- 凡是可作用于for循环的对象都是Iterable类型；
- 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；
- 集合数据类型如`list`、`dict`、`str`等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。

## 对yield的总结

（1）通常的`for..in...`循环中，`in`后面是一个数组，这个数组就是一个可迭代对象，类似的还有链表，字符串，文件。他可以是a = [1,2,3]，也可以是a = [x*x for x in range(3)]。

它的缺点也很明显，就是所有数据都在内存里面，如果有海量的数据，将会非常耗内存。

（2）生成器是可以迭代的，**但是只可以读取它一次**。因为用的时候才生成，比如`a = (x*x for x in range(3))`。**!!!注意这里是小括号而不是方括号**。

（3）`生成器（generator`）能够迭代的关键是他有`next()方法`，工作原理就是通过重复调用next()方法，直到捕获一个异常。

（4）带有`yield`的函数不再是一个普通的函数，而是一个生成器generator，可用于迭代

（5）`yield`是一个类似`return`的关键字，迭代一次遇到yield的时候就返回yield后面或者右面的值。而且下一次迭代的时候，从上一次迭代遇到的yield后面的代码开始执行

（6）**`yield`就是return返回的一个值，并且记住这个返回的位置。下一次迭代就从这个位置开始。**

（7）带有yield的函数不仅仅是只用于for循环，而且可用于某个函数的参数，只要这个函数的参数也允许迭代参数。

（8）`send()`和`next()`的区别就在于send可传递参数给yield表达式，这时候传递的参数就会作为yield表达式的值，而yield的参数是返回给调用者的值，也就是说send可以强行修改上一个yield表达式值。

（9）`send()`和`next()`都有返回值，他们的返回值是当前迭代遇到的yield的时候，yield后面表达式的值，其实就是当前迭代yield后面的参数。

（10）第一次调用时候必须先next（）或send（）,否则会报错，send后之所以为None是因为这时候没有上一个yield，所以也可以认为next（）等同于send(None)



## 示例

**使用生成器编写自己的修饰函数对 numbers 里面所有的偶数求和**

```python
def even_only(numbers):
    for num in numbers:
        if num % 2 == 0:
            yield num


def sum_even_only_v2(numbers):
    '''对 numbers 里面所有的偶数求和'''
    result = 0
    for num in even_only(numbers):
        result += num
    return result

```

将 numbers 变量使用 even_only 函数装饰后， sum_even_only_v2 函数内部便不用继续关注“偶数过滤”逻辑了，只需要简单完成求和即可。



在网站中，有一个每 30 天执行一次的周期脚本，它的任务是是查询过去 30 天内，在每周末特定时间段登录过的用户，然后为其发送奖励积分。

代码如下：

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200503144355.png"/>

上面这个函数主要由两层循环构成。外层循环的职责，主要是获取过去 30 天内符合要求的时间，并将其转换为 UNIX 时间戳。之后由内层循环使用这两个时间戳进行积分发送。

**整个循环体其实是由两个完全无关的任务构成的：“挑选日期与准备时间戳” 以及 “发送奖励积分”**。

某日，产品找过来说，有一些用户周末半夜不睡觉，还在刷我们的网站，我们得给他们发通知让他们以后早点睡觉。于是新需求出现了：“**给过去 30 天内在周末凌晨 3 点到 5 点登录过的用户发送一条通知”。**

在计算机的世界里，我们经常用“**耦合**”这个词来表示事物之间的关联关系。上面的例子中，“挑选时间”和“发送积分”这两件事情身处同一个循环体内，建立了非常强的耦合关系。

为了更好的进行代码复用，我们需要把函数里的“挑选时间”部分从循环体中解耦出来。而我们的老朋友，“**生成器函数**”是进行这项工作的不二之选。

使用生成器函数解耦循环体

要把 “挑选时间” 部分从循环内解耦出来，我们需要定义新的`生成器函数` `gen_weekend_ts_ranges()`，专门用来生成需要的 UNIX 时间戳：

```python
def gen_weekend_ts_ranges(days_ago, hour_start, hour_end):
    """生成过去一段时间内周六日特定时间段范围，并以 UNIX 时间戳返回
    """
    for days_delta in range(days_ago):
        dt = datetime.date.today() - datetime.timedelta(days=days_delta)
        # 5: Saturday, 6: Sunday
        if dt.weekday() not in (5, 6):
            continue

        time_start = datetime.datetime(dt.year, dt.month, dt.day, hour_start, 0)
        time_end = datetime.datetime(dt.year, dt.month, dt.day, hour_end, 0)

        # 转换为 unix 时间戳，之后的 ORM 查询需要
        ts_start = time.mktime(time_start.timetuple())
        ts_end = time.mktime(time_end.timetuple())
        yield ts_start, ts_end

```

有了这个生成器函数后，旧需求“发送奖励积分”和新需求“发送通知”，就都可以在循环体内复用它来完成任务了：

```python
def award_active_users_in_last_30days_v2():
    """发送奖励积分"""
    for ts_start, ts_end in gen_weekend_ts_ranges(30, hour_start=20, hour_end=23):
        for record in LoginRecord.filter_by_range(ts_start, ts_end):
            send_awarding_points(record.user_id, 1000)


def notify_nonsleep_users_in_last_30days():
    """发送通知"""
    for ts_start, ts_end in gen_weekend_ts_range(30, hour_start=3, hour_end=6):
        for record in LoginRecord.filter_by_range(ts_start, ts_end):
            notify_user(record.user_id, 'You should sleep more')

```

